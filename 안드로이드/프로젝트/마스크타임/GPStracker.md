# GPStracker

Service 이용



### 권한 추가

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```



### 클래스 

- 추상클래서 Service와 위치정보를 받아오기 위한 Listenr인터페이스 클래스 LocationListener를 상속한다.
- GPS나 네트워크 사용 유무, 얼마나 한번씩 데이터를 업데이트 할 것인지에 대한 변수들을 만든다.
  - GPS :  가장 정확하지만 실외에서만 제대로 작동, 배터리 소모 심함, 빠르게 위치계산 x
  - 안드로이드 네트워크 위치 프로바이더 : 통신사의 cell tower와 와이파이 신호 위치를 통해 실내와 실외에서 모두 측정가능
    - 빠르고 베터리 소모 적지만 정확성이 떨어짐 
- 실질적으로 위치정보를 알아오는 클래스는 LocationManager이다.(변수지정)

```java
public class GPSTracker extends Service implements LocationListener {
    private static final GPSTracker instance = new GPSTracker();

    Location location;
    double latitude;
    double longitude;

    private static final long MIN_DISTANCE_CHANGE_FOR_UPDATES = 10;
    private static final long MIN_TIME_BW_UPDATES = 1000 * 60 * 1;
    protected LocationManager locationManager;
...
}

```

- 사용자의 위치 정보를 얻는 것은 콜백 형식으로 이루어짐
- 위치 정보를 얻고자 LocationManager에게 requestLocationUpdate() 함수를 호출함으로써 요청을 하게 됨
  - LocationListener를 인자로 넘겨줘서 콜백함수로 사용하게 된다.



### 위치 접근을 위한 최적화 모델

- GPS의 정확성과 NETWORK의 배터리 소모를 효율적으로 조합하는 모델을 정의해놔야 사용자의 위치를 이용하는 어플리케이션으로서 위치 성능을 최적화 했다고 볼 수 있다.

- 흐름

  1. 앱을 시작
  2. 위치 프로바이더들로부터 사용자의 위치 정보를 listen하기 시작
  3. 사용자의 위치를 추정하여 다양한 소스로부터 들어온 정보를 정확한 것으로 보유하고 필텉링
  4. 위치 프로바이더로부터 listen을 중지
  5. 최근의 추정한 위치 사용

- 앱 시작 -> GPS와 Network 위치 프로바이더 listen -> Cache 해둔 위치 정보 사용 -> Cell-ID 기반의 위치 정보 갱신(Cache 위치 폐기) -> WiFi 기반의 위치 갱신(Cell-ID 기반의 위치 폐기) -> GPS 위치 갱신(WiFi 기반의 위치 폐기) -> GPS와 Network 위치 프로바이더 listen 중지 -> GPS 위치 정보 Cache

  - 최근에 받아온 정보를 이용하면서,**가장 정확한 위치 정보 데이터를 최대한 유지**하고 계속 새롭게 갱신해나간다는 것이다. 그리고 계속 위치 정보를 받아오는 것이 아니라 **listen하는 것을 잠시 중지 시킴**으로써 배터리 소모 문제를 해결하고자 한 것이다. 이러한 모델을 사용하고자 한다면 앱의 특성에 따라 다양한 결정들을 해야할 것이다.

- 언제 listen을 할것인가?

  - 앱을 시작하자마자 위치를 가져오거나 사요자가 특정한 기능을 실행 했을 때 시작하는 것이 좋다.
  - 지속적인 위치 정보의 갱신은 배터리 소모의 주된 원인이므로 조심해야하지만, 짧은 위치 정보읙 ㅐㅇ신은 정확성이 떨어지므로 trade-off를 잘 계산해야한다.

- 이전 수집했던 위치를 이용

  - listener가 위치를 가져올 때 까지 시간이 너무 많이 걸릴 때에는 캐쉬되어있던 위치를 가져와서 사용
  - `getLastKnownLocation(string)`함수 사용

- 언제 listen을 멈출 것인가

  - 언 시작하느냐 보다 중요한 것은 위치 수집을 언제 멈출것인가@
  - 지도를 이용하는 앱들이 수지하는 기능에만 초점을 맞춰 배터리 소모를 고려하지 않고 있겠지만, 이것을 고민해야하는 것은 지도 관련 앱을 개발하느데 있어 가장 기본!

- **데이터 정확성과 배터리 소모 조절하기**

  : 데이터 정확성과 배터리 소모는 서로 트레이드 오프가 이루어지기 때문에 적절한 선에서 정확성과 배터리 소모를 잘 고려해야할 것이다. 정확성이 어느정도 나왔다면, 대신 배터리 소모를 줄일 수 있는 몇 가지 방법들이 있다.

  - - **위치를 listen하는 시간을 조절**: 위치를 더 적은 시간동안 수집하게 된다면 위에서 더 좋은 위치 값을 선정할 때 들어오는 위치값들이 줄어들게 되므로 정확성은 조금 떨어지겠지만 배터리 소모는 아낄 수 있을 것이다.
    - **위치 프로바이더들이 위치를 업데이트하는 주기를 조절**: 위치를 listen하는 중에도 수집되는 rate을 조절하면 배터리 소모를 줄일 수 있지만 정확성이 조금 떨어질 수 있을 것이다. 앱에서 얼마나 위치가 중요한가에 따라 조절을 하면 좋을 것이다. 업데이트 주기는 requestLocationUpdates의 최소 주기와 위치를 수집하게될 변환 거리를 설정함으로써 간단하게 조절할 수 있다.
    - **사용하는 위치 프로바이더를 조절**: 구현하는 앱이 어디서 자주 이용되는가, 얼마나 높은 정확성을 요구하는가 등에 따라 네트워크 프로바이더나 GPS 프로바이더 중 하나만 선택하거나 정확성이 필요하다면 둘다 선택할 수 있을 것이다. 하나만 선택한다면 역시 배터리 소모가 줄어들겠지만 정확성이 조금 떨어질지도 모른다.



