# Permission

마스크타임 프로젝트에서는 Splash에서 권한 허가를 받고 만약 거부시 앱을 종료 시켰다.

- Retrofit을 통해 네트워크를 활용하여 사용자 위치 근처 판매처를 알아와야 했기 때문에 권한이 꼭 필요했다.
- 필수권한 : 네트워크, 위치 권한





### 권한

- 앱에서 사용자 기기에 접근하여 사용자의 정보를 얻기 위해 얻는 것
- 안드로이드 6.0 (마시멜로우) 이후에 카메라 사용이나 내부 메모리 사용 등에 대한 권한 사용 시점에, 사용자에게 직접 얻로고정책 수정

- 두가지 권한으로 나뉨 `일반 권한`과 `위험 권한`
  - 위험 권한에 대해서만 사용자에게 허가를 받으면 됌
- ContextConpact, ActivityCompat등의 클래스에 권한 요청 관련된 메서드가 추가됨



### Runtime Permission

- 마시멜로우 이전 : 사용하는 권한들을 AndroidManifrest.xml에 선언해두고 사용자가 **앱을 설치하는 시점에 허가**를 받으면 됐다.
- 마시멜로우 이후 : 앱에서 해당 권한이 필요할 때마다 사용자로부터 권한 허가를 받도록 변경
  - 설정 > 애플리케이션 > 앱이름 > 권한을 통해 언제든지 권한 허용/거부 가능
  - SDK 23 이상



### 권한을 가지고 있는지 체크

- Manifest.xml에 필요한 권한 등록
- `ContextCompat.checkSelfPermission(context, Manifiest.permission.ACCESS_COARSE_LOCATION)` 메소드를 활용해서 체크



### 권한 요청

```java
public static void requestLocationPermission(Activity activity) {
        ActivityCompat.requestPermissions(activity, new String[]{
                Manifest.permission.ACCESS_COARSE_LOCATION,
                Manifest.permission.ACCESS_FINE_LOCATION}, 100);
    }
}
```

- ActivityCompat 클래스에서 `reuqestPermissions()`를 활용하여 사용자에게 권한 허가를 요청하는 다이얼로그를 띄운다.

```
		@Override
    public void onRequestPermissionsResult(final int reqCode,
                                           @NonNull final String[] permissions,
                                           @NonNull final int[] grantResults) {
        if (PermissionUtil.allPermissionsGranted(this))
            Preference.getInstance().changeBoolean(Preference.INIT, true);
        startMainActivity();
        super.onRequestPermissionsResult(reqCode, permissions, grantResults);
    }
```

- splash 화면에서 권한 허가 요청 후 