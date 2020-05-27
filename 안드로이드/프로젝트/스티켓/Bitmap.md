# BitMap?

- Loading Large Bitamp Efficiently?
  - JPEG나 PNG는 압축포맷이기 때문에 비트맵으로 디코딩 될 때 파일의 사이즈보다 더 많은 메모리를 사용하게 된다.
  - 압축되지 않은 이미지의 용량은 픽셀 갯수*bytes이다.
  - 1920*1080  픽셀이면서 32bit인 이미지
    - 이경우 1920px * 1080px *4bytes으로 수렴되기 때문에 약 8메가 정도 된다. (약 8,294,400 bytes)
  - 이래서 JPEG 파일일 때, 일반적으로 200KB ~ 1MB가 된다.
- 큰 크기의 비트맵을 효과적으로 로딩하려면?
  - 이미지가 이미지 뷰보다 많이 크면 이미지의 퀄리티를 낮추지 않고 다운 샘ㅍ플링 하여 메모리를 절약



#### Simple Image Load vs Sampled Image Load

- 540*960 픽셀 이미지뷰로 실험
- bitmap 로드시 31.6MB 메모리 필요 , 다운샘플링시 2MB 메모리 사용 -> 16배 절감



### Bitmap object OutOfMemorry

- 안드로이드 장비의 메모리는 무한정이 아니다
  - 하나의 앱에 허용하는 메모리는 `16MB`이하를 사용하도록 하고 있다.

- 이미지는 메모리를 굉장히 많이 잡아먹는다 
  - 2592 * 1936 픽셀의 사진이 생겨나는데 ARGB_8888로 셋팅
  - 한 픽셀당 4바이트를 사용해서 결과적으로 19MB를 차지하게된다.
- 여러개의 이미지를 한방에 로드하려는 경우가 많다.
  - ListView, GrideView, ViewPager
  - 스크롤에 대비해 미리 이미지들을 가지고 있어야 되는 경우도 많다.



### 효율적인 로드

- 큰 이미지를 효율적으로 보여주는 방법
  - 원래 이미지의 해상도를 떨어뜨린다.
  - BitmapFactory.Options의 `inJustdecodeBounds=true`세팅
- 멀티쓰레드로 이미지를 읽는다.
  - 메인 UI 쓰레드에서 읽어들이는 게 아니라 AsyncTask를 사용하여 읽어들이기
- 이미지를 캐싱
  - ListView, GrideView,,,,
  - LRU 캐시!

​	