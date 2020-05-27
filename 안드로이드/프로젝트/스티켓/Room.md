# Room

### Room 이란?

- Room은 ORM (Object Relational Mapping) 라이브러리 이다.
- Room은 데이터베이스의 객체를 자바 or 코틀린 객체로 매핑
- SQLite의 추상 레이어 위에 제공되고 있으며 SQLite 모든 기능을 제공하면서 편한 데이터 베이스의 접근을 허용한다.



### Room과 SQLite의 차이

- SQLite의 경우 쿼리에 대한 에러를 컴파일에 확인하는 것이 없지만 ROOM은 컴파일 도중 SQL에 대한 유효성을 검사한다.
- Schema가 변경 되었을경우 SQL 쿼리를 수동으로 업데이트 해야하지만 Room의 경우는 쉽게 해결이 가능합니다.
- SQLite 경우 Java 데이터 객체를 변경하기 위해 많은 상용구 코드를 사용해야 하지만 Room의 경우 ORM 라이브러리가 상용구 코드 없이 매핑가능
  - 상용구 코드 : 수정하지 않거나 최소한의 수정만을 거쳐 여러곳에 필수적으로 사용되는 코드
- Room의 경우 LiveData와 RxJava를 위한 Observaion으로 생성하여 동작할 수 있지만 SQLite는 그러지 못함



### Room의 구성요소

- **Database** : 데이터베이스 보유자
- **Entity** : Database 내의 테이블
- **Dao** : 데이터베이스에 엑세스 하는데 사용되는 메소드들을 가지고 있다. select, insert, delete, join 등
  - 인터페이스로 구현



### 주의사항

room을 이용할 때 쿼리의 호출을 MainThread에서 할 경우 에러발생

- 많은 양의 쿼리를 하다보면 오랜기간 UI가 동작하지 않을 수 있기 때문에 막혀있다.
- UI 스레드 이외 스레드 에서 동작해야한다