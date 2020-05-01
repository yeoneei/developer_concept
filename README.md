# developer_interview
2020 상반기 면접 대비 레파지토리

# List
- 운영체제
- 네트워크
- 데이터베이스
- 디자인 패턴
- 자료구조
- 알고리즘
- 자바
- 자바스크립트
- 안드로이드
- 스프링
- 시큐리티
- 기타

# 운영체제
- [시스템콜](./운영체제/systemcall.md)
- [시스템 부트](./운영체제/systemboot.md)
- [프로세스](./운영체제/process.md)
- 스레드
- 프로세스와 스레드의 차이(Process vs Thread)
- 멀티 프로세스 대신 멀티 스레드를 사용하는 이유
- Thread-safe
- 동기화 객체의 종류
- 세마포어
- [데드락](./운영체제/deadlocks.md)
    - 교착상태(데드락, Deadlock)의 개념과 조건
- 뮤텍스와 세마포어의 차이
- 동기와 비동기
- 프로세스 동기화
- [CPU 스케줄링](./운영체제/cpuScheduling.md)
- [메모리 관리 전략](./운영체제/memory.md)
- Main memory
    - Address binding 
    - Segmentation
    - Paging
    - TLB
- 가상 메모리
    - Virtual Memory
    - Demand Paging
    - Page Replacement 알고리즘 (FIFO, LRU, ,,,)
    - Thrashing
- CPU 스케줄링
- 캐시의 지역성
- File System (Inode, journaling, allocation, caching)
- 사용자 수준 스레드와 커널 수준 스레드
- 외부 단편화와 내부 단편화
- Context Switching
- Swapping
- Disk 스케줄링 (HDD vs SSD)
- IO system 
    - Polling , interrupt, blocking, non blocking

# 네트워크
- OSI 7계층
- TCP/IP의 개념
- TCP와 UDP
- TCP와 UDP의 헤더 분석
- TCP의 3-way-handshake와 4-way-handshake
    - TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?
    - 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?
    - 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?
- HTTP와 HTTPS
- HTTP 요청/응답 헤더
- CORS란
- REST
- GET 메서드와 POST 메서드
- 쿠키(Cookie)와 세션(Session)
- DNS
- REST와 RESTful의 개념
- 소켓(Socket)이란
- Socket.io와 WebSocket의 차이
- Frame, Packet, Segment, Datagram

# 데이터베이스
- 데이터베이스 풀
- 정규화(1차 2차 3차 BCNF)
- 트랜잭션(Transaction) 이란
- 트랜잭션 격리 수준(Transaction Isolation Level)
- Join
- SQL injection
- Index란
- Statement와 PrepareStatement
- RDBMS와 NoSQL
- 효과적인 쿼리 저장
- 옵티마이저(Optimizer)란
- Replication
- 파티셔닝(Partitioning)
- 샤딩(Sharding)
- 객체 관계 매핑(Object-relational mapping, ORM)이란
- java JDBC

# 디자인 패턴
- 디자인 패턴의 개념과 종류
- Singleton 패턴
- Strategy 패턴
- Template Method 패턴
- Factory Method 패턴
- MVC1 패턴과 MVC2 패턴
- 어댑터 패턴
- 빌더
- 위임 패턴
- 데코레이터 패턴

# 자료구조
- Array
- LinkedList
- HashTable
- Stack
- Queue
- Graph
    - Directed
    - Undirected
    - Adjacency matrix
    - Adjacency list
    - BFS, DFS
- search
    - Preorder, inorder,postorder
- Binary search
- Tree
    - Binary search Tree
- Bitwise operations (비트연산)
- 그래프(Graph)와 트리(Tree)의 차이점
- Heap / Priority Queue(힙으로)/ Binary Heap
- AVL
- Red-Black Tree
- B+ Tree
- Tries

# 알고리즘
- BigO
- sort
    - Selection
    - Insertion
    - Heapsort
    - Quicksort
    - Merge sort
- DFS와 BFS의 차이
- PNp, NP-Complete and Approximate algorithm
- Fibonacci에서의 세 가지(Recursion, Dynamic Programming, 반복) 방식에 대한 시간복잡도와 공간복잡도 차이
- 정렬 알고리즘의 종류와 개념
- 최소 신장 트리(MST, Minimum Spanning Tree)란
    - Kruskal MST 알고리즘
    - Prim MST 알고리즘
- Disjoint set, ion find
- 벨만포드
- 스케쥴링알고리즘

# 자바
- java 프로그래밍이란
- Java SE와 Java EE 애플리케이션 차이
- java와 c/c++의 차이점
- java 언어의 장단점
- java의 접근 제어자의 종류와 특징
- java의 데이터 타입
- Wrapper class
- OOP의 4가지 특징
    * 추상화(Abstraction), 캡슐화(Encapsulation), 상속(Inheritance), 다형성(Polymorphism)
- OOP의 5대 원칙 (SOLID)
- OOP 왜 쓰는지?
- 객체지향 프로그래밍과 절차지향 프로그래밍의 차이
- 객체지향(Object-Oriented)이란
- java의 non-static 멤버와 static 멤버의 차이
    * Q. java의 main 메서드가 static인 이유
- java의 final 키워드 (final/finally/finalize)
- java의 제네릭(Generic)과 c++의 템플릿(Template)의 차이
- java의 가비지 컬렉션(Garbage Collection) 처리 방법
- java 직렬화(Serialization)와 역직렬화(Deserialization)란 무엇인가
- 클래스, 객체, 인스턴스의 차이
- 객체(Object)란 무엇인가
- 오버로딩과 오버라이딩의 차이(Overloading vs Overriding)
- Call by Reference와 Call by Value의 차이
- 인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)
- JVM 구조
- Java Collections Framework
    * java Map 인터페이스 구현체의 종류
    * java Set 인터페이스 구현체의 종류
    * java List 인터페이스 구현체의 종류
- Annotation
- String, StringBuilder, StringBuffer
- 동기화와 비동기화의 차이(Syncronous vs Asyncronous)
- java에서 '=='와 'equals()'의 차이
- java의 리플렉션(Reflection) 이란
- GC
- 람다
- 제너릭
- Shallow Copy, Deep Copy
- Stream

# 자바스크립트
- JavaScript Event Loop
- 함수 선언식과 함수 표현식
- 화살표 함수(Arrow Function)
- 향상된 객체 리터럴(Enhanced Object Literals)
- Modules
- 디스트럭처링(Destructuring)
- 전개 연산자(Spread Operator)
- 호이스팅(Hoisting)
- Closure
- this
- Promise
- Async/Await

# 안드로이드
- 4대 컴포넌트
- Fragment , intent , context
- 핸들러, 루퍼, 메세지 큐 , ui thread , 비동기 라이브러리
- View lifecycle
- Context
- MVC, MVP, MVVM 왜 쓰는가. 각 패턴의 장단점.
- 가능하면 Jetpack

# 스프링
- 스프링 프레임워크란
- Spring, Spring MVC, Spring Boot의 차이
- Container란
- IOC(Inversion of Control, 제어의 역전)란
- MVC 패턴이란
- DI(Dependency Injection, 의존성 주입)란
- AOP(Aspect Oriented Programming)란
- POJO
- DAO와 DTO의 차이
- Spring JDBC를 이용한 데이터 접근
- ORM 이란

# 시큐리티
- 대칭키와 비대칭키 차이
- 패스워드 암호화 방법
- SQL Injection 공격
- CSRF 공격
- XSS 공격

# 기타
- TDD란
- 웹 브라우저에서 서버로 어떤 페이지를 요청하면 일어나는 일련의 과정을 설명
    * Ex. url에 'www.naver.com' 을 입력했다. 일어나는 현상에 대해 아는대로 설명하라.
- 컴파일러와 인터프리터
- 분산락
- 프레임워크와 라이브러리의 차이
- 64bit CPU와 32bit CPU 차이
- CVS, SVN, Git
- Git Branch 종류(5가지)
- 웹 서버(Web Server)와 웹 어플리케이션 서버(WAS)의 차이
- 애자일 방법론이란
- Servlet과 JSP
- Memcached와 Redis의 차이
- Maven과 Gradle의 차이
- 부동소수점
- 유니코드
- Endiannes


