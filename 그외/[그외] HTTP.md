# HTTP 파헤치기

### HTTP 란?

- Hyper Text Transfer protocol로, 인터넷에서 데이터를 주고 받을 수 있는 프로토콜(규칙)
-  프로토콜에 맞추어 데이터를 주고 받는다



### HTTP 특징

- 상태가 없다. (statelss)
  - 데이터를 주고 받기 위한 가각의 데이터 요청이 서로 독립적으로 관리된다.
  - 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다.
  - 서버는 세션과 같은 별도의 추가 정보를 관리하지 않아도 되고, 다수의 요청 처리 및 서버의 부하를 줄일 수 있는 성능 상의 이점이 생긴다.
- TCP/IP 통신 위에서 작동하며 기본 포트는 80번



### URL

- URL(Uniform Resouce Locators) : 서버에 자원을 요청하기 위해 입력하는 영문 주소
- http:// www.domain.com:1234/path/to/resource?a.a=b&x=y
  - http:// : 프로토콜
  - www.domain.com : host
  - 1234 : port
  - path/to/resource? : resource path 
  - a=b&x=y : query



### HTTP 요청 메소드

- GET : 존재하는 자원에 대한 요청
- POST : 새로운 자원을 생성
- PUT : 존재하는 자원에 대한 변경 (전체 갱신)
- PATHCH : 존재하는 자원에 대한 변경 (부분 갱신)
- DELETE : 존재하는 자원에 대한 삭제
- HEAD : GET방식과 동일하지만, 응답에 BODY가 없고 응답코드와 HEAD만 응답
  - 웹서 정보확인, 헬스 체크, 버전확인, 최종 수정일자 확인등의 용도로 사용
- OPTIONS : 서버 옵션들을 확인하기 위한 요청
  - CORS 에서 사용



### HTTP의 멱등성

- 멱등(idempotent) : 같은 작업을 계속 반복해도 같은 결과가 나오는 경우
  - GET은 모든 요청에 반환되는 응답이 동이랗다
  - POST vs PUT : POST는 insert개념이고 PUT은 update 개념
    - POST 는 멱등하지 않고 PUT은 명등하다
    - POST는 자원의 변화가 생긴다

| HTTP 메소드 | 요청 Body 여부 | 응답 Body 여부 | 안전   | 멱등   | 캐시가능 |
| ----------- | -------------- | -------------- | ------ | ------ | -------- |
| GET         | 아니요         | 예             | 예     | 예     | 예       |
| POST        | 예             | 예             | 아니오 | 아니오 | 예       |
| HEAD        | 아니요         | 아니요         | 예     | 예     | 예       |
| PUT         | 예             | 예             | 아니요 | 예     | 아니요   |
| DELETE      | 아니요         | 예             | 아니요 | 예     | 아니요   |
| PATCH       | 예             | 예             | 아니요 | 아니요 | 예       |
| OPTIONS     | 선택           | 예             | 예     | 예     | 아니요   |



### HTTP 응답 캐시

- 응답을 캐싱하도록 구성하여 유사한 요청이 애플리케이션 호스트로 저달되지 않도록 할 수 있다.
- 애플리케이션 호스트는 비용이 높은 데이터베이스 쿼리나 자주 요청되는 파일들에 대한 응답을 캐시 할 수도 있다.
- 웹 서버의 응답은 메모리에 캐싱하고, 애플리케이션 캐시는 로컬 인메모리에 저장하거나 캐시 서버 위에서 실행되는 레디스와 같은 인메모리 데이터베이스를 설계할 수 있다.
- **HTTP 헤더를 통한 브러우저 캐싱**
  - 모든 브라우저는 HTML 페이지, 자바스크립트 파일 및 이미지와 같은 웹 문서의 임시 저장을 위해 HTTP 캐시(웹 캐시)의 구현을 제공하고 있다.
  - HTTP 헤더 지시문을 제공하여 브라우저가 응답을 캐싱할 수 있는 시기와 지속 기간을 지시할 때 사용
  - 리소스가 로컬 캐시로 부터 빠르게 로드되기 때문에 점점 하드웨어 성능이 급격하고 있는 최근 잘만 활용한다면 엄청난 속도 제공
  - 각 서버단에서 맞는 HTTP 헤더를 내려주어 브라우저에게 응답 캐시를 언제 얼마나 보유할지 가이드 하기만 하면 된다.



### HTTP 상태코드

- 2xx - 성공
- 3xx - 리다이렉션
  - 클라이언트가 이전 주소로 데이터를 요청하여 서버에 URL로 리 다이렉트를 유도하는 경우
- 4xx - 클라이언트 에러
  - 유효하지 않은 자원 요청, 요청 권한이 잘못된 경우
- 5xx - 서버에러
  - 서ㅔ서 오류가 난 경우





### HTTP Keep Alive

- HTTP 구조
  - HTTP는 Connectionless 방식으로 연결을 매번 끊고 새로 생성하는 구조
  - 이는 Network 비용 측면에서 최초 연결을 하기 위해 많은 비용을 소비하는 구조
- Keep Alive란?
  - HTTP/1.1부터는 이미 연결되어 있는 TCP를 재사용하는 Keep-Alive라는 기능을 Default로 지원한다.
  - Handshake과정이 생략되므로 향상을 기대할 수 있다.
- Keep Alive 필요 이유
  - 서버 자원은 무한정이 아니다.
  - 그렇기 떄문에 이러한 접속을 계속 유지하는 것은 Serverdprp thstlfdmf qkftodtlzlsek.
  - 즉, 서버와 연결을 맺을 수 있는 Socket은 한정되어 있고 연결이 오래 지속되면 다른 사람들이 연결을 못하기 되는 상황이 닥치게 된다.



##### Keep Alive 사용하지 않았을 때

```
$ telnet pungjoo.com 80
Trying 121.124.124.74...
Connected to pungjoo.com.
Escape character is '^]'.
GET / HTTP/1.0
Host: pungjoo.com
HTTP/1.1 302 Moved Temporarily
Date: Tue, 15 Jan 2008 01:33:56 GMT
Set-Cookie: JSESSIONID=B4329BFEDB1363BB90FCFA9568DDBF0B; Path=/
Location: http://pungjoo.com/servlet/com.pungjoo.blog2005.Action
Content-Type: text/html;charset=EUC-KR
Content-Length: 0
Connection: close

Connection to pungjoo.com closed by foreign host.
```

- Response 메세지와 함께 `Connection to pungjoo.com closed by foreign host` 메시지가 노출된다.
- 즉 Response 메시지와 함께 커넥션이 종료됨을 알 수 있다.



##### Keep Alive 사용

```
$ telnet pungjoo.com 80
Trying 121.124.124.74...
Connected topungjoo.com.

// [1]
GET / HTTP/1.1
Host: pungjoo.com
Connection: Keep-Alive
HTTP/1.1 302 Moved Temporarily
Date: Tue, 15 Jan 2008 01:36:26 GMT
Set-Cookie: JSESSIONID=2B3EE2FE56868BD9588A7B55C405974B; Path=/
Location: http://pungjoo.com/servlet/com.pungjoo.blog2005.Action
Content-Type: text/html;charset=EUC-KR
Content-Length: 0
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive

// [2]
GET / HTTP/1.1
Host: pungjoo.com
Connection: Keep-Alive
HTTP/1.1 302 Moved Temporarily
Date: Tue, 15 Jan 2008 01:36:35 GMT
Set-Cookie: JSESSIONID=E4E3D9B9CFE3693DC5AFC3D6ABDE5564; Path=/
Location: http://pungjoo.com/servlet/com.pungjoo.blog2005.Action
Content-Type: text/html;charset=EUC-KR
Content-Length: 0
Keep-Alive: timeout=5, max=99
Connection: Keep-Alive

// [3]
Connection to pungjoo.com closed by foreign host.
```

- Telnet 연결은 한번만 이뤄졌고 GET Method는 2번([1],[2]) 요청했다.
- 이렇게 할 수 있는 이유는 **Keep Alive** 때문
- [1] : Keep-Alive: timeout=5, max=100
  - 추가적으로 100번 Request 가능
  - timout 5초
- [3]  : 5초간 지속되고 timout이 된다
  - Server / Client의 연결이 5초간 유지



##### 특징

- Keep Alive 유지시간은 연결된 Socket에 I/O Access가 마지막으로 종료된 시점부터  정의된 시간까지 Access가 없더라도 세션을 유지하는 구조
- 즉 정의된 시간 내에 Access가 이러우진다면 계속 연결 상태를 유지할 수 있게 된다.



##### 장점

- 연속적인 4개의 request가 있을 때 원래는 4번 연결하고 4번 끊는 작업을 반복해야 하는데 하나의 지속적인 커넥션으로 request를 주고 받을 수 있기 때문에 시간이 단축된다.



##### 단점

- 정적 자원으로만 구성된 웹 서버에 Keep Alive를 사용할 경우 약 50%의 성능 향상을 보인다
- 바쁜 서버 환경에서 Keep Alive 기능을 사용할 경우 모든 요청마다 연결을 유지해야 하기 때문에 프로세스 수가 기하 급수적으로 늘어나 Max Client 값을 초과하게 된다.
- 따라서 메모리를 많이 사용하게 되며 성능저하의 원인이 된다.
- 즉 대량 접속 시 효율이 떨어짐



##### 문제점

- Keep-Alive 기능은 설계상 문제가 있다. -> 해결 X 그래서 HTTP/1.1에는 제외
- 하지만 한번 생성된 커넷견을 재사용한다는 개념은 유지
- 멍청한(Dumb) 프록시
  - 일반적으로 HTTP는 클라이언트와 서버사이에 프록시 서버, 캐시 서버 등과 같은 중개 서버가 놓이는 것을 허락한다.
  - 여기서 프록시 서버가 멍청할 경우 문제 발생
  - 멍청한 프록시는 Connection 헤더를 이해하지 못하고 해당 헤더들을 삭제하지 않고 요청을 그대로 다음 프록시에 전달
  - 개발자들은 Proxy-Connection이라는 헤더를 사용하는 차선책을 제시
    - 브라우저에서 일반적으로 전달하는 Connection헤더 대신 비표준인 Proxy-Connection 확장 헤더를 프록시에게 전달한다.
    - 프록시가 Proxy-Connection 헤더를 무조건 전달하더라도 웹 서버는 그것을 무시하기 때문에 문제가 되지 않는다.

##### Keep Alive 사용법

- HTTP/1.0+ (Keep Alive를 사용하기 위해 설정)

```
lient(Browser)는 http 1.1을 준수하고 이해 할 수 있다고
Request에 Connection헤더에 Keep-Alive 값을 넣어 Server에 전송한다.
ex) Connection : Keep-Alive, ... 

Request를 받는 Server는 
Kepp Alive 기능을 활성화하고 
Keep Alive Timeout을 설정한다.
```

- HTTP/1.1 (Keep Alive를 끊기 위한 설정)
  - Default로 동작을 하므로 설정에 신경을 쓰지 않아도 된다.



##### 주의사항

- 모든 TCP 세션을 무한정 유지할 수는 없으므로 Timeout 및 Max 설정을 통해 관리되어야한다.





