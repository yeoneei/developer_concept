# Glide



### 안드로이드에서 사진을 보여주는 경우

-  ImageView에 사진을 띄우고자 하는 경우는 여러가지
  1. 안드로이드 앱 안의 drawable폴더의 리소스를 보여주는 경우
  2. 안드로이드 디바이스 안에 저장되어있는 사진을 보여주는 경우(갤러리 혹은 기타 내부 사진)
  3. 이미지 URL을 로드해서 보여주고자 하는 경우

- 1,2번의 경우는 안드로이드 기기 내부의 리소스를 불러오는 작업이므로 예외사항도 적고 실제 구현도 복잡하지 않다.

- 3번처럼 이미지의 URL인 경우 http클라이언트를 이용해서 ImageView에 보여주어야 하는경우는 고려해야할 사항이 많다.
  - 로딩 실패처리, 재시도처리, Out of Memory, 캐시,병렬처리, 디코딩, 이미지재활용 등등



### 주의 해얄점 (naver)

- 이미지 로딩을 구현 할 때 HTTP 통신을 안정되게 구현하고, 비트맵으로 디코딩하면 메모리가 넘치거나 새지 않도록 주 해야한다.네워크 호출과 디코딩은 단순히 백그라운드 스레드에서 동작하는 것만으로는 충분하지 않고 더 적극적으로 병렬성을 활용해야한다. 화면 회전, 전환, 스크롤 때 반복적인 요청이 가지 않도록 이미지를 캐시하고, 불필요해진 요청은 빠른 시점에 취소해서 더 나은 UI 반응을 제공하면서 자원을 절약해한다.



### 이미지 로딩 라이브러리 워크 플로

- 이미지 전처리 -> 이미지 로딩 -> 이미지 디코딩 -> 이미지 후처리 -> 보여주기
- 이미지 전처리 : 이미지를 로딩하기 전에 섬네일이나 진행 상황을 보여주기 위한 단계
- 이미지 로딩 : 캐시나 네트워크에서 이미지를 가져오는 단계
- 이미지 디코딩 : BitmapFactory를 이용하여 이미지를 비트맵 형식으로 변환하고 크기, 회전, 품질 등을 변환
- 이미지 후처리 : 이미지에 애니메이션이나 모서리를 둥글게 하는 등의 효과를 적용하는 단계
- 보여주기 : UI 스레드에서 이미지를 보여주는 단계



### 이미지 라이브러리들

- Universal Image Loader(UIL)
  - 가장 많이 쓰임
  - 여러가지 Custom하게 변경할 수 있도록 많은 옵션 제공
- Picasso
  - UIL 이후 최근에 가장 널리 쓰이고 있는 이미지 로딩 라이브러리
- Glide
  - 구글에서 개발해서 밀고있던 volly 이후에 2014년에 공개된 라이브러리
  - 썸네일 보기, GIF로딩, 동영상 스틸 보기 기능 지원
- Freso
  - facebook에서 공개한 이미지 라이브러리



### Glide 사용법

1. Gradle 추가

```xml
implementation 'com.github.bumptech.glide:glide:4.9.0'
annotationProcessor 'com.github.bumptech.glide:compiler:4.9.0'
```

2. 사용법

```
Glide.with(this).load("http://www.selphone.co.kr/homepage/img/team/3.jpg").into(imageView);
```

3. 유용한 함수
   - override() : 지정한 이미지의 크기만큼만 불러올수 있다.
     - 이를 통해 이미지 로딩 속도를 최적화 할 수 있다.
   - placeholder() : 이미지를 로딩하는 동안 처음에 보여줄 placeholder 이미지 지정가능
   - error() : 실패했을 경우 실패 이미지 지정 가능
   - thubnail() : 지정한 %비율 만큼 미리 이미지를 가져와서 보여줌
     - 0.1f로 지정했다면 실제 이미지 크기중 10%만 먼저 가져와서 흐릿하게 보여줌
   - asGif() : gif 로딩가능



### 성능비교

- 메모리, 속도 값은 총 6장의 이미지를 리스트뷰에 띄우는 방식으로 3회 진행하여 평균값 계산
- 반복 스크롤 부분은 50장의 이미지를 리스튜 뷰에 띄웠고 메모리를 정리하기 직전 최대 사용량을 구함
  - https://gun0912.tistory.com/17



### Picaso vs Glide

- 함수 명명 방식은 똑같음
- with() 함수에서 picaso는 Context만 지원하고 Glide는 context 뿐만아니라 다른 객체들도 사용가능
  - Fragment같은 경우 getActivity()를 해줄 필요 없이 this로 해주어도 Glide 사용가능
- 화질
  - 1920X1080 이미지를 768X432크기의 이미지에 로드한 경우 Picasso가 이미지 화질이 더 좋음
  - Picasso는 Bitmap 포맷을 ARGB_8888로 사용하고 Glide는 RGB_565사용
    - 대신 Picasso가 메모리 사용량이 더 많음
    - 메모리 용량보다 화질이 더 중요하면 Glide를 ARGB_8888로 변경 가능
    - 같은 비트맵 포맷에서 Picassork 훨씬 더 많은 메모리 사용량을 보임 
    - Picasso는 원본 이미지를 메모리로 가져와서 실시간으로 리사이징에 할당
    - Glide는 바로 작은 사이즈로 메모리에 가져와서 이미지 할당
- 이미지 캐시 정책
  - 만약 1920x1080 이미지를 다시 384x216크기의 ImageView로 로드한다고 할 경우 Picasso는 이미 원본 이미지를 그대로 가지고 있지만 Glide는 또하나의 384x216 크기의 이미지파일을 캐시
  - Glide는 같은 이미지를 다른 크기의 ImageView에 로드한다는 이유로 2번의 이미지 다운로드와 리사이징 작업이 필요
  - 만약 Glide에서 원본크기의 이미지를 캐시하도록 설정하고싶은경우 아래와 같이 캐시정책을 추가
  - Glide는 이미지를 최대한 빨리 로드해 주는데 최적화 되어있다!

- 