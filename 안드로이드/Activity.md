# Activity

Activity 클래스는 Android의 중요 구성요소로 Activity이 실행되고 결합되는 방식은 플랫폼 애플리케이션의 기본 요소다.  Android 시스템은 수명 주기의 특정 단계에 해당하는 특정 콜백 메서드를 호출하여 Activity 인스턴스 코드를 시작한다.



### Activity 개념

한 앱이 다른 앱을 호출할 때 호출 앱은 다른 앱을 전체적으로 호출하는 것이 아니라 다른 앱의 Activity를 호출한다. 이런 방식으로 Activity는 사용자의 상호작용을 위한 진입점 역할을 한다.

- UI가 그리는 창을 제공
- 앱에서 하나의 화면을 구현
- Main Activty >> 앱을 실행할 때 표시되는 첫번째 화면
- Activity는 일관된 사용자 환경을 형성하기 위해 함께 작동하지만 각 Activity는 단지 느슨하게 결합되어 있다.
  - 앱 Activity간에는 최소한의 종속성만 존재한다.
- Activity를 사용하려면 manifest에 관련 정보를 등록하고 Activity Life Cycle을 적절히 관리해야한다



### Manifest 선언

< activity > 요소를 < application > 요소의 하위 요소로 추가한다.

```
    <manifest ... >
      <application ... >
          <activity android:name=".ExampleActivity" />
          ...
      </application ... >
      ...
    </manifest >

```

- `android:name` 클래스 이름을 지정하는 필수 속성
- 선언한 후 Activity이름을 변경하면 안됌



### 인텐트 필터 선언

- 인텐트 필터 : 명시적 요청과 암시적 요청으로 나뉨
  - 명시적 요청 : 'Gmail 앱에서 이메일 보내기 활동으로 이메일 시작'
  - 암시적 요청 : '작업을 할 수 있는 활동으로 이메일 보내기 화면 시작'
- 시스템 UI에서 사용자에게 작업을 실행할 때 어떤 앱을 사용할지 묻는 메세지가 표시되면 인텐트 필터가 적용 된 것

```
    <activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
        <intent-filter>
            <action android:name="android.intent.action.SEND" />
            <category android:name="android.intent.category.DEFAULT" />
            <data android:mimeType="text/plain" />
        </intent-filter>
    </activity>
    
```

```
    // Create the text message with a string
    Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.setType("text/plain");
    sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
    // Start the activity
    startActivity(sendIntent);
    
```



### 권한 설정

manifest의 < activity >태그를 사용하여 특정 활동을 시작할 수 있는 앱을 제어할 수 있다. 상위 activity와 하위 activity 활동이 모두 manifest에서 동일한 권한을 가지고 있지 않으면 하위 활동을 실행할 수 없다. 상위 활동에서  < uses-permission >요소를 선언 할 때에는 각 하위 활동에 일치하는 < uses-permission > 요소가 있어야 한다.



### Activity Life Cycle

##### Activity Life Cycle이 필요한 이유

- 사용자가 앱을 사용하는 도중에 전화가 걸려오거나 다른 앱으로 전환될 때 비정상 종료를 막을 수 있다.
- 앱을 활발하게 사용하지 않은 경우 시스템 리소스 소비를 막을 수 있다.
- 앱에서 나갔다 들어 올때 사용자의 진행 상태 손실을 막을 수 있다.
- 가로, 세로 방향 회전의 경우 비정상 종료되거나 진행상태 손실을 막을 수 있다.



##### Activty Life Cycle 개념

![lifecycle](./img/activity_lifecycle.png)

- 사용자 Activity를 떠나기 시작하면 시스템은 Acitivity를 해체할 메서드를 호출 한다.
- 이 때 Activity는 여전히 메모리 안에 남아있으며 포그라운드로 다시 돌아 올 수 있다.
- Acitivty로 다시 돌아오는 경우 Activity는 사용자가 종료한 지점에서 다시 시작된다.
- 백그라운드에서 실행 될 떄 Activity를 실행할 수 없다.



##### Activity Life Cycle 콜백

사용자가 앱을 탐색하고 앱에서 나가고, 앱으로 다시 돌아오면 앱의 Activity인스턴스는 생명주기 안에서 서로 다른 상태를 통해 전환된다. Activity 클래스는 Activity상태 변화를 알아 차릴 수 있는 여러 콜백을 제공한다.

-  `onCreate()` : Activity가 생성할 때 실행되는 콜백, 필수 구성요소를 초기화, 필수적으로 구현

  - 데이터 바인딩, `ViewModel`과 연결

  - 전체 생명 주기중 한번 만 발생해야 함

  - `saveInstanceState` 매개변수를 수신하는데 Activity의 이전 저장 상태가 포함된 Bundle객체

    - 처음 생성되면 null을 가짐

  - setContentView()를 호출하여 UI 레이아웃을 정의해야하며 이 작업이 가장 중요

  - `onCreate()`메서드가 실행을 완료하면 시작됨 상태가 되고 연달아 `onStart()`, `onResume()`메서드를 호출한다

  - `onCreate()`가 완료되면 다음 콜백은 항상 `onStart()`이다.

  - ```
    TextView textView;
    
    String gameState;
    
    @Override
    public void onCreate(Bundle savedInstanceState) {
        // 부모 클래스의 onCreate을 부른다, 뷰 상속
        super.onCreate(savedInstanceState);
    		
    		// 인스턴스 상태 복구
        if (savedInstanceState != null) {
            gameState = savedInstanceState.getString(GAME_STATE_KEY);
        }
        
        // UI연결
        setContentView(R.layout.main_activity);
    		//초기화
        textView = (TextView) findViewById(R.id.text_view);
    }
    
    //이전에 사용하여 저장된 인스턴스가 있는 경우에만 호출
    // onCreate -> 일부 상태르르 복원, 선택적 복원
    // onStart가 완료 된 후 사용 가능 한 상태
    @Override
    public void onRestoreInstanceState(Bundle savedInstanceState) {
        textView.setText(savedInstanceState.getString(TEXT_VIEW_KEY));
    }
    
    // detory될 때 호출, 인스턴스 상태를 저장
    @Override
    public void onSaveInstanceState(Bundle outState) {
        outState.putString(GAME_STATE_KEY, gameState);
        outState.putString(TEXT_VIEW_KEY, textView.getText());
    
        super.onSaveInstanceState(outState);
    }
    ```

- `onStart()` :  `onCreate()`가 종료되면 활동은 '시작됨'상태로 전환되고 활동이 사용자에게 표시된다

  - `onStart()`가 호출되면 Activity가 사용자에게 보이게 되고 포그라운드에 보내 상호작용할 수 있도록 준비한다.
    - UI 관리 코드 초기화
  - 포그라운드로 나와서 대화형이 되기 위한 최종 준비를 하는 작업
  - `onStart()`완료 후 이 상태에 머무르지 않고 `onResume()`메소드 호출

- `onResume()` : 사용자와 상호작용할 때

  - 포그라운드에 표시되고 시스템이 이 콜백을 호출
    - 포커스가 떠날 때가지 이 상태 유지 (ex. 전화가 오거나, 다른 Activity로 이동)
  - 이 시점에서 activity는 활동 스택의 맨 위에 있으며 모든 사용자의 입력을 캡쳐한다.
  - 핵심기능은 대부분 `onResume()`메서드로 구현된다.
  - 방해되는 이벤트가 발생하면 일시 정지 상태에 들어가고 `onPause()` 을 호출 한다.
    - 일시 정지에서 재개 됨 상태로 돌아오면 `onResume()`를 호출한다.  그래서 `onPause()`중에 해제하는 구성요소를 초기화 하고 상태가 전환될 때 필요한 다른 초기화 작업도 수행해야 한다.

- `onPause()` : activity가 포커스를 잃고 '일시중지'상태로 전환

  - 사용자가 activty를 떠나는 것( activity가 포그라운드에 있지 않게 되었다는 것 ) , 소멸되는 것은 아님

  - 사용자가 활동을 떠나고 있으며 activity가 조만간 '중지됨', '다시 시작됨'상태로 전환됨을 나타냄

  - 뒤로 또는 최근 버튼을 탭할 때 발생

  - '일시중지' 상태에서 UI 업데이트가 가능하다

    - 내비게이션 지도, 또는 미디어 플레이어 재생 활동을 표시
    - UI 같은경우 onPause()대신 onStop()을 이용하는 것이 좋음
      - 멀티 윈도우 모드에서는 여전히 보이는 상태가 됨, 멀티 윈도우 모드를 더욱 잘 지원하기 위해서!

  - 사용자 데이터 저장, 네트워크 호출, 데이터 베이스 트랜잭션 실행 할 때는 사용하면 안된다.

    - 그럼 뭘사용해??? >> onStop()?

    시스템 리소스, 핸들러(ex. GPS) 또는 배터리 수명에 영향을 미칠 수 있는 모든 리소스 해제 가능

  - `onPause()`가 실행 완료되면 다음 콜백활동이  `onStop()`  이나 `onResume()` 가 된다.

- `onStop()` : activity가 더 이상 표시되지 않을 때 호출

  - activity가 제거되거나 '다시 시작됨'상태로 전환중이고 중지된 활동을 다루고 있기 때문에 발생 할 수 있다.
    - 더 이상 표시되지 않는다.
  - 관련 UI작업 계속 진행
  - CPU를 비교적 많이 소모하는 작업을 종료
    - DB 이런거
  - activity가 중단됨 상태에 들어가면 activity 객체는 메모리 안에 머무르게 된다.
    - View객체의 현재 상태 기록 등
  - activity가 중단되면 시스템은 항상 activity가 포함된 프로세스를 소멸 시킬 수 있다.
    - 중단된 동안 시스템이 프로세스를 소멸시켜도 Bundle(키-값 쌍의 blob)에 있는 View 객체가 그대로 유지되어 사용자가 다시 돌아오면 이를 복원함

  - `onRestart()`나 `onDestroy()`을 호출함

- `onRestart()` : '중지됨' 상태의 활동이 다시 시작되려고 할 때 시스템은 이 콜백을 호출

  - 활동 상태를 복원
  - 항시 `onStart()`가 호출됨

- `onDestroy()` : activity가 제거되기 전에 이 콜백 호출

  - activity가 수신하는 마지막 콜백
  - 일반적인 activity 또는 activity이 포함된 프로세스가 제거 될 떄 모든 리소스를 해제하도록 구현
  - 이 콜백이 호출 되는 두가지 경우
    - 사용자가 activity를 완전히 닫거나 finish()가 호출되어 activity가 종료되는 경우
    - 구성 변경(ex. 기기 회전, 멀티 윈도우 모드)로 인해 시스템이 일시적으로 activity를 소멸시키는 경우
      - 구성 변경으로 다시 생성될 경우 ViewModel은 그대로 보존되어 다음 Activity 인스턴스에 전달되므로 추가 작업이 필요하지 않다.
      - ViewModel은 onCleared()메소드를 호출하여 소멸 전 모든 데이터를 정리해야한다.

  - 아직 해제되지 않은 모든 리소스를 해제해야한다.

  

### Activity 상태 및 메모리 제거

RAM에 여유 공간이 필요할 때 프로세스를 종료한다. 특정 프로세스를 종료할 가능성은 그 시점의 프로세스 상태에 따라 달라진다.

| 종료될 가능성 | 프로세스 상태                                   | Activity 상태          |
| ------------- | ----------------------------------------------- | ---------------------- |
| 최소          | 포그라운드 (포커스가 있거나 포커스 가져올 예정) | 생성됨, 시작됨, 재개됨 |
| 높음          | 백그라운드 (포커스 상실)                        | 일시 정지              |
| 최대          | 백그라운드 (보이지 않음)                        | 정지                   |
|               | 비어있음                                        | 소멸됨                 |

메모리 공간 확보를 위해 Activity를 직접 종료하지 않는다. Activity를 실행하는 프로세스를 종료하여 모든 작업을 함께 소멸시킨다. 



### 임시 UI 상태 저장 및 복원

사용자는 구성이 변경되더라도 Activity의 UI 상태가 그대로 유지되기를 원한다. 시스템의 제약으로 인해 Activity가 소멸되면 ViewModel, onSaveInstanceState() 및 로컬 저장소를 결합하여 사용자의 임시 UI상태를 보존해야한다.

- 가벼운 데이터는 `onSaveInstanceState()`만으로 UI 상태 보존
  - 직렬화 / 역직렬화 비용을 발생시키기 때문에 `ViewModel`과 같이 사용해야한다.



##### 인스턴스 상태

- `finish()`나 사용자가 `백버튼`을 눌렀을 때는 인스턴스에 대한 시스템과 사용자 콘셉트가 모두 영구적으로 사라짐
  - 추가 작업 x
- 시스템 제약(ex. 구성 변경 또는 메모리 부족)으로 인해 Activity를 소멸시킬 경우, 실제 Activity인스턴스는 사라지더라도 시스템에 존재했다는 정보는 남아있음
  - 저장된 데이터 세트를 사용하여 해당 Activity의 새로운 인스턴스를 생성
- 이전 상태를 복원하기 위해 사용하는 데이터를 `인스턴스 상태`라고 하며 이는 Bundle객체에 저장된 키-값 쌍의 컬렉션
  - 기본적으로 시스템은 Bundle 인스턴스 상태를 사용하여 Activity레이아웃의 각 View객체에 대한 정보를 저장함
  - Activity 뷰의 상태를 복원하기 위해서는 android:id특성으로 제공되는 고유 ID가 각 뷰마다 있어야한다.
- `Bunlde` 객체는 메인 스레드에서 직렬화 되어야하고 시스템 프로세스 메모리를 사용하므로 소량의 데이터를 보존하는데 적합
  - 그 이상의 데이터를 보전하려면 `onSaveInstanceState()`메서드, `ViewModel`클래스로 데이터를 보존하는 복합적인 방법을 사용해야한다.



##### onSaveInstacneState()를 사용하여 간단하고 가벼운 UI상태 저장

- ListView위젯의 스크롤 위치와 같은 Activity의 뷰 계층 구조에 대한 임시 정보를 저장
- 추가적인 인스턴스 상태정보를 저장하려면 `onSaveInstanceState()`를 재정의
  - 기본 구현에서 뷰 계층 구조의 상태를 저장하고자 한다면 상위 클래스 구현을 호출해야한다.
- 예상치 못하게 소멸될 경우 저장되는 Bundle 객체에 키-값 쌍을 추가해야한다.
- 명시적으로 finish()가 호출되는경우에는 `onSaveInstanceState()`가 호출되지 않는다.



##### 저장된 인스턴스 상태를 사용하여 Activity UI 상태 복원

- `Bundle`로부터 저장된 인스턴스 상태 복구
  - `onCreate()`과 `onRestoreInstanceState()` 메서드 둘다 Bundle을 수신한다
- `onCreate()`는 시스템이 activity의 새 인스턴스를 생성하든, 이전 인스턴스를 재생성하든 상관없이 호출되므로 읽기를 시도하기 전에 Bundle이 null인지 확인해야한다.

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); // Always call the superclass first

    // Check whether we're recreating a previously destroyed instance
    if (savedInstanceState != null) {
        // Restore value of members from saved state
        currentScore = savedInstanceState.getInt(STATE_SCORE);
        currentLevel = savedInstanceState.getInt(STATE_LEVEL);
    } else {
        // Probably initialize members with default values for a new instance
    }
    // ...
}
```



```
public void onRestoreInstanceState(Bundle savedInstanceState) {
    // Always call the superclass so it can restore the view hierarchy
    // 상위 클래스구현을 호출하여 기본 구현에서 뷰 계층 구조의 상태를 복원할 수 있도록 해야함
    super.onRestoreInstanceState(savedInstanceState);

    // Restore state members from saved instance
    currentScore = savedInstanceState.getInt(STATE_SCORE);
    currentLevel = savedInstanceState.getInt(STATE_LEVEL);
}
```

- `onRestoreInstanceState()`는 `onStart()`메서드 다음에 호출
  - 복원할 저장 상태가 있을 경우에만 호출함 >> null확인 필요 없음!



### Activity간 이동

##### 다른 Activity에서 Activity시작하기

- `startActivity()`, `startActivityForeResult()` 메서드를 사용하여 시작
  - 두가지 모두 Intent객체 전달
- `Intent`는 시작하고자하는 Activity를 정확히 나타내거나, 수행하고자하는 작업의 유형을 설명
  - `Intent`에는 소량의 데이터 포함 가능



##### startActivity()

- 새로 시작된 Activity가 결과르 반환할 필요가 없을 경우

```
Intent intent = new Intent(this, SignInActivity.class);
startActivity(intent);
```

- 기기에 있는 다른 애플리케이션이 제공하는 Activity를 대신 활용하여 동작을 수행하게 할 수도 있다

```
Intent intent = new Intent(Intent.ACTION_SEND);
intent.putExtra(Intent.EXTRA_EMAIL, recipientArray);
startActivity(intent);
```

- 이메일 애플리케이션의 activity가 시작됨



##### startActivityForResult()

- Activity가 종료될 때 결과를 반환 받고자 할 떄 사용
  - 사용자가 연락처 목록에서 어떤 사람을 선택 할 수 있도록 하는 Activity -> activity 종료시 선택한 사람 반환
- `startActivityForResult(Intent, int)` 메서드 호출
  - 정수 매개변수가 호출을 식별
- `onActivityResult(int, int, Intent)`메서드를 통해 반환
  - 하위 Activity 존재시 `setResult(int)`를 호출하여 Activity로 데이터 반환

```
public class MyActivity extends Activity {
     // ...

     static final int PICK_CONTACT_REQUEST = 0;

     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
             // When the user center presses, let them pick a contact.
             startActivityForResult(
                 new Intent(Intent.ACTION_PICK,
                 new Uri("content://contacts")),
                 PICK_CONTACT_REQUEST);
            return true;
         }
         return false;
     }

     protected void onActivityResult(int requestCode, int resultCode,
             Intent data) {
         if (requestCode == PICK_CONTACT_REQUEST) {
             if (resultCode == RESULT_OK) {
                 // A contact was picked.  Here we will just display it
                 // to the user.
                 startActivity(new Intent(Intent.ACTION_VIEW, data));
             }
         }
     }
 }
```

- `RESULT_CANCELED`(비정상 종료), `RESULT_OK`를 반환한다
- Intent객체를 반환할 수도 있다.



##### Activity 조정

- 한 Activity가 다른 Activity를 시작하면, 둘 모두 수명주기가 전환됨
  - 다른 Activity가 생성되는 동안 첫번째 Activity는 작동을 중단하고 일시정지 됨 또는 정지됨 상태로 들어간다.
- Activity가 디스크 또는 다른 곳에 저장된 데이터를 공유하는 경우 첫번 째 Activity는 두번째 Activity가 생성되기 전에` 완전히 중단되지 않는다`.
  - 두 번째 Activity 시작 과정이 첫번째 Activity의 중단과정과 겹쳐 일어난다.
-  Activity A, Activity B의 작업순서
  - Activity A의 `onPause()`실행
  - Activity B의 `onCreate()`,  `onStart()`,  `onResume()` 메서드가 순차적으로 실행 
    - 포커스가 B로 옮겨짐
  - Activity A가 더이상 화면에 표지 되지 않는 경우 `onStop()` 메서드가 실행된다.

콜백 순서를 예측할 수 있기 때문에 전환되는 정보를 관리 할 수 있다.



### Activity 상태 변경

사용자에 의해 트리거되고 시스템에 의해 트리거되는 다양한 이벤트로 인해 Activity가 상태가 변경될수 있음

- 구성 변경 발생
  - ex : 가로 모드, 세로모드
  - activity가 제거되고 다시 생성
  - 원래 활동 인스턴스에서는 `onPause()`, `onStop()`, `onDestory()`가 트리거 이후 새 인스턴스에서 `onCreate()`, `onStart()`, `onResume()` 콜백이 트리거
  - ViewModel, onSaveInstanceState(), 영구 로컬 저장소의 조합을 활용하여 UI 상태 유지
  - 멀티 윈도우 : 사용자에게 표시되는 앱이 두개더라도 상호작용하고 있는 앱만 포커스를 가진다
    - 사용자가 앱을 전환 할 떄마다 두 메서드가 전환
- activity 또는 대화상자가 포그라운드로 나옴
  - activity를 부분적으로 가리면 가려진 activity는 포커스를 읽고 '일시중지'상태로 전환
    - `onPause()`를 호출
  - 다시 포커스를 얻으면 `onResume()` 호출
  - 사용자가 최근 사용 또는 홈 버튼을 탭하면 시스템은 현재 활동이 가려진 것처럼 동작한다.
- 사용자가 백버튼을 누름
  - `onPause()`, `onStop()`,` onDestory()`콜백을 거치며 전환
  - activity가 제거될 뿐만 아니라 백 스택에서도 삭제
  - `onSaveInstacneState()`콜백이 실행되지 않는다.
  - `onBackPressed()`메소드를 재정의하면 'confirm-quit'대화 상자와 같은 일부 맞춤 동작을 구현 할 수 있음
    - 사용시 `super.onBackPressed()`를 호출해야함
- 시스템에 앱 프로세스를 종료
  - UI 상태를 저장하고 다시 복구함



### Activity Test 

androidX테스트 라이브러리 사용, 공부하고 정리하기



