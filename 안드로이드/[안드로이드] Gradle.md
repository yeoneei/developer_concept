# Android Gradle이란

### 빌드 배포 도구

- Android Studio와 빌드 시스템이 서로 독립적이다.
- Android Studio는 편집만 담당할 뿐, 빌드는 모두 Gradle을 통해 모두 수행
- 빌드 절차와 IDE가 분리되어 있기 때문에 프로젝트를 더 깔끔하게 관리할 수 있게 됨
  - Android Studio와 Gradle빌드 설정이 동기화 되지 않으면 에러



### build.gradle

- 모듈의 빌드 방법이 정의된 빌드 스크립트
- 빌드에 사용할 SDK 버전 부터 시작하여 애플리케이션 버전, 라이브러리 등 다양한 항목 설정 가능

1. Apply plugin : 'com.android.application'
   - 안드로이드 플로그인 사용을 gradle에 적용
   - top-level에 선언되어야함
2. android {...}
   - 안드로이드와 관련된 빌드 설정은 이곳에서 세팅
3. complieSdkVersion, buildToolsVersion
   - 앱 컴파일시 사용할 SDK 버전
   - 사용할 빌드툴의 버전
4. defaultConfig{...}
   - AndroidManifest.xml에서 사용하는 설정들에 대해서 동적인 옵션을 주고 싶을 때 블록 내에 포함
   - versionCode나 versionName등
5. buildTypes{...}
   - dev, alpha, beta, relesase 같이 빌드 타입 종류를 지정
6. Dependencies{...}
   - 라이브러리와 같은 의존성 추가



### Gradle 의존성 분리

Gradle에서 라이브러리들을 변수화 해서 분리하는 작업 가능

- 많은 수의 라이브러리를 사용하고 있을 때 라이브러리 버전을 변경하거나 수정하는 작업을 하면 시간이 많이 소요 되기 때문