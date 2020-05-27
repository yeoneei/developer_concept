# FaceTracking

ML Kit Facedetection 사용! 

### 시작하기

- Firebase dependency 추가하기
- 실시간 애플리케이션에서 얼굴 인식을 하는 경우 입력 이미지 전체 크기를 고려해야 할 수도 있다.
- 이미지가 작을 수록 빠르게 처리 되고 지연시간을 줄이려면 낮은 해상도에서 이미지를 캡쳐 해야한다.



### 시작 하기전 안드로이드 카메라 알아야 할 부분

- FPS : 1초에 몇 프레임씩 찍는지

- ```
  setRequestedFps(29.97f)
  ```

- 이렇게 설정해 줬는데 29.97f는 무슨 기준?



### 권한 설정

- ```xml
  <uses-permission android:name="android.permission.CAMERA" />
  ```

- ```xml
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ```

- 위에는 카메라 권한 밑에는 저장 권한



### 사용한 이유

- ML kit Face traking 라이브러리에서 얼굴을 인지하고 눈,코,입,귀,뺨과 같은 LANDMARK를 인지해 주기 때문에
- 카메라 권한 허가 후 권한이 설정되면 카메라에 페이스 디텍션을 빌드한다.

```java
FaceDetector detector = new FaceDetector.Builder(context)
                .setLandmarkType(FaceDetector.ALL_LANDMARKS)
                .setClassificationType(FaceDetector.NO_CLASSIFICATIONS)
                .setMode(FaceDetector.FAST_MODE)
                .setProminentFaceOnly(true)
                .build();
```



### preveiw화면

- `CameraSurfaceView` 를 이용하여 preview 화면 구현



### 사진 찍기, 캡쳐

- 



### 과정

- 사용자가 아바타에 사진을 매핑
  - 이때 위치, 크기, 회전 정도, 반전 여부와 같이 저장 (이미지는 bitmap으로 저장)
  - Bitmap : SD카드가 아닌 deviece의 메모리에 이미지를 저장!
    - 비트맵을 사용한 이유? 네트워크 없이 카메라 앱을 사용할 때 자신이 가지고 있는 스티커들을 불러 올 수 있어야한다
      - S3를 사용해 다른 곳에 저장할 경우 네트워크가 필요하기 때문에 기기 내부에 저장하기 위해 Bitmap을 사용하였다.

- 매핑 한 것을 카메라 프리뷰 화면에서 클릭하면 내부 DB(Room)에서 가져와서 매칭
  - 해당하는 조합을 DB에서 가지고옴
  - 크기, 위치, 회전여부에 대한 정보에 맞게 해당 이미지를 세팅
  - Sticker View를 이용해 landmark 위치와 stickeveiw 좌표차이를 계산하여 sticker뷰에 계속 해서 갱신해서 오려줌



### 위기

- 스티커 매칭
  - 처음에 저장되는 포인트가 스티커의 왼쪽 상단이여서 중앙 좌표값을 다시 계산해서 위치 시켜줌



### 예상질문

##### 이 라이브러리는 무엇인가?

- face traking 라이브러리

##### 왜 이 라이브러리를 사용하였는가?

- Open cv로 구현되어 있는 라이브러리는 많았지만 android에 적용하기에는 라이브러리가 많지 않았고 frace tracker등 여러 tracking 라이브러리를 사용해 보았지만 사용자의 모션을 잘 따라가지 못하는 현상이 발생

##### 이 오픈 소스의 장점은?

- 사용자의 모션을 실시간으로 잘 따라가고 Landmark를 알려준다.

##### 이 오픈소스 사용 방법은?

- 카메라 세팅시 face detector라는 개체에 landmark설정을 true로 하고 객체를 빌드해 준다.

##### 오픈소스 내부 구조와 로직을 명확히 알고있나?

- 어떻게 구현이 됬는지는 잘 모르겠다. 찾아보고싶은,,,

##### 오픈소스 쓰지 않고 구현이 가능 한가

- 모델을 학습시키고 안드로이드에 연동을 시키면 할 수 있을지도,,,

