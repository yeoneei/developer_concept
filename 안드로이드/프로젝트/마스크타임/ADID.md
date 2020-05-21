# ADID

android ADID란 GAID(Google Advertising ID)를 의미한다.

- 구글에서 제공하는 Google Play Service의 API를 이용하여 ad ID를 얻을 수 있는데
- Google Play Service가 없는 디바이스에서는 사용 불가
- GAID는 유저 식별용으로 사용하기에 아주 적합



### 사용방법

- build.gradle에 들어가 dependecies에서 다음과 같이 선언

```
dependencies {
    ...
    implementation 'com.google.android.gms:play-services:11.0.4'
    implementation 'com.google.android.gms:play-services-auth:11.0.4'
    ...
}
```

- **AdvertisingIdClient 클래스**의 **getAdvertisingIdInfo(Context).getId()** 메소드를 통해 ADID를 얻는다.

  **(주의할 점은 해당 작업을 백그라운드 스레드에서 수행해야한다는 점이다.)**

- Task 실행하듯이 execute()로 실행



### UUID vs ADID

- UUID : 기기 고유 식별 번호
- 근데 왜 ADID 사용?
  - 우리 프로젝트에서는 사용자가 앱을 삭제하면 사용자가 앱이 싫어서 삭제한 것인데 데이터가 남아 있는 것이 맞지 않다고 판단하여 ADID 사용
  - 로그인 기능을 없애고 싶어서 사용!

