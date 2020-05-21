# Java와 Kotlin

### Kotlin 이란?

- JetBrains에서 2011년 공개한 프로그래밍 언어
- JVM, Android, 브라우저를 위한 정적 타입의 프로그래밍 언어
- Null 안전성
- coroutiones 지원
  - 코루틴은 특정 위체에서 실행을 일시 중단하고 다시 시작할 수 있는 여러 진입점을 허용하여 비선점 멀티 태스킹을 위해 서브루틴을 일반화 하는 컴퓨터 프로그램 구성요소
  - 서브루틴은 진입점과 중단점이 1개인 코루틴이라고 정의할 수 있다.
  - thread와의 차이점
    - os가 스케쥴링 / 유저가 스케쥴링
    - 선점형 / 비선점형
    - (멀티)스레드는 최소 2개 / 코루틴은 스레드가 1개(코루틴은 함수다)
- Java와 상위 호환



### Android와 kotlin

- java와 100%호환 -> Android API그대로 사용가능
- Ant, Maven, Gradle build 시스템 사용가능
- Android studio를 통한 Java -> kotlin 변환 도구 제공





### Java vs Kotlin

- 함수표현

  - ```java
    // Java
    [public/private/protected] ReturnType 함수이름(Type 변수들){
      [return]
    }
    
    // Kotlin
    [public/private/protected] fun 함수이름(Type 변수들): ReturnType{
      [return]
    }
    ```

    - 기존 자바 표현식과 다르게 함수 이름 앞에 fun 이라는 예약어를 붙이고 함수이름을 적어 선언

- 변수 표현

  - ```java
    // Java
    [public/private/protected] [static/final] Type 변수이름 [= <초기값>]
    // Kotlin
    [public/private/protected] [var/val] 변수이름[:Type] [= <초기값>]
    
    // Java
    String va1 = "hello"; // mutable, read and write possible
    final String va2 = "world"; // immutable, read only
    // Kotlin
    var va1 = "hello" // mutable, read and write possible
    val va2 = "world" // immutable, read only
    ```

    - var과 val로 변수 선언
      - val은 immutaable / var은 mutable

- Null 처리

  - ```java
    // Java
    String nu1 = "hello";
    nu1 = null;
    
    // Kotlin
    var nu1 :String = "hello"
    n1 = null // <---error:Null can not be a value of non-null type String
    var nu2 :String? = "hello"
    n2 = null // possible
    ```

    - 변수 type 뒤에 물음표를 붙임으로써 null 허용/ 비허용 여부 선언 가능
      - null이 비허용된 변수 경우 null이 절대 못들어감
    - nullable 프로퍼티는 null할당 가능
    - Non-nullable은 컴파일 에러
    - 코틀린에서 NPE(null point exception)이 발생하지 않을 것 같지만 자바 라이브러리를 쓰는경우 NPE가 발생 할 수 있다.
      - 자바에는 Non-nullable 타입이 없기 때문에 자바 라이브러리를 사용할 때 nullable타입으로 리턴
    - `!!`는 객체가 null이 아닌것을 보장한다.
      - null이면 NPE 발생

- Class 표현법

  - ```java
    // Java
    public class Person {
      private final String name;
      public Person(String name){
        this.name = name;
      }
    }
    
    // Kotlin
    class Person constructor(val name: String){}
    ```

    - constructor 예약어를 통해 생성과 동시에 지정되어야하는 매개변수에 대해 선언 가능

- VO 표현법

  - ```java
    // Java
    public class User {
      private String name;
      private int age;
      public int getAge(){
        return age;
      }
      public void setAge(int age){
        this.age = age;
      }
      public Stirng getName(){
        return name;
      }
      public void setName(String name){
        this.name = name;
      }
    }
    
    // Kotlin
    data class User(var name:String, var age:Int)
    // automatically make .equals(), .hashCode(), .toString(), .copy(), .componentN()
    ```

    - 기존 java에서 value object를 선언하게 되면 oop의 원칙에 따라 매개변수는 private으로 감추고 getter/setter을 통해 각 변수를 접근하였으나, Kotlin에서는 데이터 클래스를 사용하여 vo로 자주 사용하는 각종 함수를 자동으로 제공



