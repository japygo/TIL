# 7장 DB를 활용해 데이터를 영구적으로 저장하기

데이터를 영구적으로 저장하고 조회할 필요가 있는데 이 문제에 대한 해결책은 데이터베이스 서버를 도입해 해결할 수 있다.
자바 진영은 JDBC라는 표준을 통해 데이터베이스와의 통신을 담당하도록 지원하고 있다.

## 7.1 회원 데이터를 DB에 저장하기 실습

### 7.1.1 실습 코드 리뷰 및 JDBC 복습

자바 진영은 데이터베이스에 대한 접근 로직 처리를 담당하는 객체를 별도로 분리해 구현하는 것을 추천한다.
이 객체를 `DAO(Data Access Object)`라고 부른다.

### 7.1.2 회원 목록 실습

### 7.1.3 개인정보 수정 실습

## 7.2 DAO 리팩토링 실습

[expert one-on-one J2EE 설계와 개발](http://www.yes24.com/Product/Goods/428809)(로드 존슨 저/자바 유저스 번역팀 역, 정보문화사/2004년) 책의 "4장 J2EE 프로젝트의 설계 기술 및 코딩 표준"을 보면 컴파일타입 Exception과 런타임 Exception을 사용해야 되는 가이드라인을 다음과 같이 제시하고 있다.

- API를 사용하는 모든 곳에서 이 예외를 처리해야 하는가?
  예외가 반드시 메소드에 대한 반환 값이 되어야 하는가?
  이 질문에 대한 답이 "예"일 경우 컴파일타임 Exception을 사용해 컴파일러의 도움을 받는다.
- API를 사용하는 소수 중 이 예외를 처리해야 하는가?
  이 질문에 대한 답이 "예"일 경우 런타임 Exception으로 구현한다.
  API를 사용하는 모든 코드가 Exception을 catch하도록 강제하지 않는 것이 좋다.
- 무엇인가 큰 문제가 발생했는가?
  이 문제를 복구할 방법이 없는가?
  이 질문에 대한 답이 "예"라면 런타임 Exception으로 구현한다.
  API를 사용하는 코드에서 Exception을 catch하더라도 에러에 대한 정보를 통보 받는 것 외에 아무것도 할 수 있는 것이 없다.
- 아직도 불명확한가?
  그렇다면 런타임 Exception으로 구현하라.
  Exception에 대해 문서화하고 API를 사용하는 곳에서 Exception에 대한 처리를 결정하도록 하라.

### 7.2.1 요구사항

JDBC에 대한 공통 라이브러리를 만들어 개발자가 SQL 쿼리, 쿼리에 전달할 인자, SELECT 구문의 경우 조회한 데이터를 추출하는 3가지 구현에만 집중하도록 해야한다.

### 7.2.2 요구사항 분리 및 힌트

1. INSERT, UPDATE 쿼리는 비슷하기 때문에 먼저 INSERT, UPDATE 쿼리를 가지는 메소드의 중복 제거 작업을 진행한다.
2. 분리한 메소드 중에서 변화가 발생하지 않는 부분(즉, 공통 라이브러리로 구현할 코드)을 새로운 클래스로 추가한 후 이동한다.
3. InsertJdbcTemplate과 UpdateJdbcTemplate의 UserDao에 대한 의존관계를 끊는다.
4. InsertJdbcTemplate과 UpdateJdbcTemplate의 구현 부분이 다른 부분이 없다. 하나를 사용하도록 리팩토링한다.
5. JdbcTemplate은 아직도 User와 의존관계를 가지기 때문에 다른 DAO 클래스에서 재사용할 수 없다. User와 의존관계를 끊는다.
6. 더 이상 JdbcTemplate은 특정 DAO 클래스에 종속적이지 않다. 이와 똑같은 방법으로 SelectJdbcTemplate을 생성해 반복 코드를 분리한다.
7. JdbcTemplate과 SelectJdbcTemplate을 보니 중복 코드가 보인다. 또한 굳이 2개의 클래스를 제공하고 싶지 않다. JdbcTemplate과 같은 한 개의 클래스만을 제공하도록 리팩토링해본다.
8. 위와 같이 SelectJdbcTemplate 클래스로 통합했을 때의 문제점을 찾아보고 이를 해결하기 위한 방법을 찾아본다.
9. SQLException을 런타임 Exception으로 변환해 throw하도록 한다. Connection, PreparedStatement 자원 반납을 close() 메소드를 사용하지 말고 try-with-resources 구문을 적용해 해결한다.
10. SELECT문의 경우 조회한 데이터를 캐스팅하는 부분이 있다. 캐스팅하지 않고 구현하도록 개선한다.
11. 각 쿼리에 전달할 인자를 PreparedStatementSetter를 통해 전달할 수도 있지만 자바의 가변인자를 통해 전달할 수 있는 메소드를 추가한다.
12. UserDao에서 PreparedStatementSetter, RowMapper 인터페이스를 구현하는 부분을 JDK 8에서 추가한 람다 표현식을 활용하도록 리팩토링한다.

## 7.3 동영상을 활용한 DAO 리팩토링 실습

- https://youtu.be/ylrMBeakVnk: DAO 리팩토링 1단계로 DAO 중복 코드에 대한 이슈 제기, 라이브러리 코드와 개발자가 구현해야 하는 코드 분리, abstract 키워드를 활용해 추상 클래스 구현, INSERT, UPDATE, DELETE 문에 대한 중복 제거 과정을 다룬다.
- https://youtu.be/zfXAZkqPH44: SELECT 쿼리에 대한 중복 코드 제거, getConnection() 메서드 중복 제거 및 테스트 코드 수정, 템플릿 메소드 패턴 적용 과정을 다룬다.
- https://youtu.be/yEHUB97B62I: 템플릿 메소드 패턴을 활용해 JdbcTemplate과 SelectJdbcTemplate 통합, 템플릿 메소드 패턴을 활용할 때의 문제점, 각 메소드를 콜백 인터페이스로 분리해 JdbcTemplate과 SelectJdbcTemplate 통합하는 과정을 다룬다.

반복적으로 발생하는 중복 로직을 상위 클래스가 구현하고 변화가 발생하는 부분만 추상 메소드로 만들어 구현하도록 하는 디자인 패턴을 템플릿 메소드(Template Method) 패턴이라고 한다.

- https://youtu.be/nkepkHJi7e8: 자바 제너릭을 활용해 캐스팅을 하지 않도록 라이브러리 구현, 가변 인자를 활용해 개발자 편의성 개선, 여러 건의 데이터를 조회할 수 있는 기능 추가, 라이브러리 코드에서 중복을 제거하는 과정에 대한 설명한다.

메소드 하나만 가지는 인터페이스를 생성한 후 필요에 따라 메소드의 인자로 전달해 앞 단계에서 발생한 문제점을 해결했다.
즉, 변화 시점이 다른 부분을 서로 다른 인터페이스로 분리함으로써 공통 라이브러리에 대한 유연함을 높일 수 있게 되었다.
이를 콜백(Callback) 인터페이스라고 부른다.

- https://youtu.be/lFTyw7Uipyo: 컴파일타임 Exception, 런타임 Exception 차이, SQLException을 런타임 익셉션으로 변환해서 처리하는 과정을 다룬다.
- https://youtu.be/0ax9jxfW9x4: 자바 8에서 추가한 람다를 적용하기 RowMapper에 FunctionalInterface 어노테이션 설정, RowMapper를 사용할 때 람도 표현식을 사용하도록 리팩토링하는 과정을 다룬다.

람다 표현식으로 사용할 인터페이스라고 지정하려면 인터페이스에 `@FunctionalInterface` 애노테이션을 추가해야 한다.

## 7.4 리팩토링 및 설명

- 콜백 인터페이스를 활용했을 때의 유연함이다.
  콜백 인터페이스는 공통 라이브러리를 만들거나 코드에 유연함이 필요한 경우 유용하게 사용할 수 있다.
  사실 콜백 인터페이스는 함수형 언어에서 함수를 메소드의 인자로 전달함으로써 유연함을 얻는 것과 같다.
  자바가 인터페이스를 사용할 수 밖에 없는 이유는 기본 단위가 함수가 아닌 클래스이기 때문이다.
- 리팩토링을 최대한 안전하게 진행해야 한다는 것이다.
  리팩토링은 새로운 기능을 추가하지 않으면서 설계를 개선하는 작업이다.
  따라서 리팩토링 과정에서 테스트를 통해 기능이 정상적으로 동작하는지 반드시 확인해야 한다.

## 7.5 추가 학습 자료

### 7.5.1 데이터베이스

- [SQL 첫걸음 : 하루 30분 36강으로 배우는 완전 초보의 SQL 따라잡기](http://www.yes24.com/Product/Goods/22744867)(아사이 아츠시 저/박준용 역, 한빛미디어/2015년)
- [Real MySQL : 개발자와 DBA를 위한](http://www.yes24.com/Product/Goods/6960931)(이성욱 저, 위키북스/2012년)
- [NoSQL : 빅 데이터 세상으로 떠나는 간결한 안내서](http://www.yes24.com/Product/Goods/8510944)(프라모드 사달게이,마틴 파울러 공저/윤성준 역, 인사이트/2013년)

### 7.5.2 디자인 패턴

- [GoF의 디자인 패턴 : 재사용성을 지닌 객체지향 소프트웨어의 핵심요소](http://www.yes24.com/Product/Goods/17525598)(에릭 감마 저/김정아 역, 프로텍미디어/2015년)
- [Head First Design Patterns : 스토리가 있는 패턴 학습법](http://www.yes24.com/Product/Goods/1778966)(에릭 프리먼 저 / 서환수 역, 한빛미디어/2005)
- [실전 코드로 배우는 실용주의 디자인 패턴](http://www.yes24.com/Product/Goods/34572809)(Allen Holub 저/송치형 역, 지앤선/2006)