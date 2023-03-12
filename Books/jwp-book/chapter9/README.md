# 9장 두 번째 양파 껍질을 벗기기 위한 중간 점검

## 9.1 자체 점검 요구사항(필수)

1. 톰캣 서버를 시작하면 서블릿 컨테이너의 초기화되는 과정에 대해 설명하라.
   1. `WebServerLauncher` 클래스의 `main` 함수가 실행된다.
   2. `Tomcat`이 실행된다.
   3. `contextPath`와 기본 경로가 지정된다.
   4. `ContextLoaderListener` 클래스가 생성된다.
   5. `jwp.sql`로 데이터베이스가 초기화된다.
   6. `DispatcherServlet` 클래스의 `init` 함수가 실행된다.
   7. `RequestMapping` 클래스가 초기화된다.
2. 톰캣 서버를 시작한 후 `http://localhost:8080`으로 접근해서 질문 목록이 보이기까지 소스코드의 호출 순서 및 흐름을 설명하라.
   1. `http://localhost:8080`으로 요청하면 `/`에 매핑되어 있는 `HomeController`을 가져온다.
   2. `questions`에 담겨져 있는 질문 목록을 가져온다.
   3. `/home.jsp`에 해당하는 view를 응답한다.
3. 질문 목록은 정상적으로 동작하지만 질문하기 기능은 정상적으로 동작하지 않는다. 질문 기능을 구현한다.
4. 로그인한 사용자만 질문이 가능하도록 수정하고 로그인한 사용자 정보를 가져와 글쓴이 이름으로 사용하도록 구현한다.
5. 답변 목록을 데이터베이스에 저장되어 있는 답변을 출력하도록 구현한다.
6. `CharacterEncodingFilter`에 애노테이션 설정을 통해 한글 문제를 해결한다.
7. `ShowContorller`는 멀티스레드 상황에서 문제가 발생할 가능성이 있는 코드이다.
   문제가 발생하지 않도록 수정하고 문제가 되는 이유를 작성한다.
   ```java
   public class ShowController extends AbstractController {
       private QuestionDao questionDao = new QuestionDao();
       private AnswerDao answerDao = new AnswerDao();
       private Question question;
       private List<Answer> answers;

       @Override
       public ModelAndView execute(HttpServletRequest req, HttpServletResponse response) throws Exception {
           Long questionId = Long.parseLong(req.getParameter("questionId"));

           question = questionDao.findById(questionId);
           answers = answerDao.findAllByQuestionId(questionId);

           ModelAndView mav = jspView("/qna/show.jsp");
           mav.addObject("question", question);
           mav.addObject("answers", answers);
           return mav;
       }
   }
   ```
   1. 서로 다른 스레드에서 `ShowController`를 거의 동시에 사용하게 된다고 가정한다.
   2. 이떄 `question`과 `answer`에 나중에 사용한 스레드의 데이터가 담기고 먼저 사용한 스레드가 해당 데이터를 사용하게 될 수 있다.
8. 답변을 추가하는 시점에 질문의 댓글 수(countOfAnswer)도 1 증가해야 한다.
9. `/api/qna/list` URL로 접근했을 때 질문 목록을 JSON 데이터로 조회할 수 있도록 구현한다.
10. 상세보기 화면의 답변 목록에서 답변을 삭제해야 한다.
11. 질문 수정이 글쓴이와 로그인 사용자가 같은 경우에만 수정이 가능해야 한다.
12. `JdbcTemplate`은 인스턴스를 여러 개 생성할 필요없이 하나만 생성하도록 구현한다.
13. 질문 삭제 기능을 구현한다. 답변이 없거나 질문자와 답변자가 모두 같은 경우 질문 삭제가 가능하다.

## 9.2 자체 점검 요구사항(선택)

14. Dao를 사용하는 모든 컨트롤러 클래스는 데이터베이스가 설치되어 있어야 하며, 테이블까지 생성되어 있는 상태에서만 테스트가 가능하다.
    데이터베이스가 존재하지 않는 상태에서도 단위 테스트가 가능하도록 구현한다.
15. 서블릿과 같이 애노테이션을 활용해 설정을 추가하고 서버가 시작할 때 자동으로 매핑이 되도록 개선해 본다.

## 9.3 자체 점검 확인

### 9.3.1 서블릿 컨테이너와 MVC 프레임워크 초기화 과정(1번)

1. 서블릿 컨테이너는 웹 애플리케이션의 상태를 관리하는 `ServletContext`를 생성한다.
2. `ServletContext`가 초기화되면 컨텍스트의 초기화 이벤트가 발생한다.
3. 등록된 `ServletContextListener`의 콜백 메소드(contextInitialized)가 호출된다.
   이 문제에서는 `ContextLoaderListener`의 `contextInitialized()` 메소드가 호출된다.
4. jwp.sql 파일에서 SQL문을 실행해 데이터베이스 테이블을 초기화한다.
5. 서블릿 컨테이너는 클라이언트로부터의 최초 요청시(또는 컨테이너에 서블릿 인스턴스를 생성하도록 미리 설정을 한다면 최초 요청 전에) `DispatcherServlet` 인스턴스를 생성한다(생성자 호출).
   이 문제에서는 `loadOnStartup` 속성이 설정되어 있기 때문에 서블릿 컨테이너가 시작하는 시점에 인스턴스를 생성한다.
6. `DispatcherServlet` 인스턴스의 `init()` 메소드를 호출해 초기화 작업을 진행한다.
7. `init()` 메소드 안에서 `RequestMapping` 객체를 생성한ㄷ.
8. `RequestMapping` 인스턴스의 `initMapping()` 메소드를 호출한다.
   `initMapping()` 메소드에서는 요청 URL과 `Controller` 인스턴스를 매핑시킨다.

### 9.3.2 첫 화면에 접근했을 때 사용자 요청부터 응답까지 흐름(2번)

1. `localhost:8080`으로 접근하면 요청을 처리할 서블릿에 접근하기 전에 먼저 `ResourceFilter`와 `CharacterEncodingFilter`의 `doFilter()` 메소드가 실행된다.
   `ResourceFilter`의 경우 해당 요청이 정적 자원(CSS, 자바스크립트, 이미지) 요청이 아니기 때문에 서블릿으로 요청을 위임한다.
2. 요청 처리는 "/"으로 매핑되어 있는 `DispatcherServlet`이므로 이 서블릿의 `service()` 메소드가 실행횐다.
3. `service()` 메소드는 요청받은 URL을 분석해 해당 `Controller` 객체를 `RequestMapping`에서 가져온다.
   요청 URL은 "/"이며, 이와 연결되어 있는 `HomeController`가 반환된다.
4. `service()` 메소드는 `HomeController`의 `execute()` 메소드에게 작업을 위임한다.
   요청에 대한 실질적인 작업은 `HomeController`의 `execute()` 메소드가 실행한다.
   `execute()` 메소드의 반환 값은 `ModelAndView`이다.
5. `service()` 메소드는 반환 받은 `ModelAndView`의 모델 데이터를 뷰의 `render()` 메소드에 전달한다.
   이 요청에서 `View`는 `JspView`이다.
   `JspView`는 `render()` 메소드로 전달된 모델 데이터를 `home.jsp`에 전달해 HTML을 생성하고, 응답함으로써 작업을 끝낸다.

### 9.3.3 스택과 힙 메모리(7번)

자바 프로그래밍에서 클래스의 인스턴스를 생성할 때 비용이 발생한다.
인스턴스를 생성하고 더 이상 사용하지 않을 경우 가비지 콜렉션 과정을 통해 메모리에서 해제하는 과정 또한 비용이 발생한다.
따라서 인스턴스를 매번 생성할 필요가 없는 경우 매번 인스턴스를 생성하지 않는 것이 성능 측면에서 더 유리하다.

매 요청마다 서로 다른 상태 값을 가지지 않기 때문에 매번 인스턴스를 생성하지 않고 인스턴스 하나를 생성한 후 재사용할 수 있다.

서블릿은 서블릿 컨테이너가 시작할 때 인스턴스 하나를 생성한 후 재사용한다.
서블릿 컨테이너는 멀티스레드 환경에서 동작한다.
즉, 멀티스레드 환경에서 여러명의 사용자가 인스턴스 하나를 재사용하고 있다.

JVM은 코드를 실행하기 위해 메모리를 스택과 힙 영역으로 나눠서 관리한다.
스택 영역은 각 메소드가 실행될 때 메소드의 인자, 로컬 변수 등을 관리하는 메모리 영역으로 각 스레드마다 서로 다른 스택 영역을 가진다.
힙 영역은 클래스의 인스턴스 상태 데이터를 관리하는 영역이다.
힙 영역은 스레드가 서로 공유할 수 있는 영역이다.

JVM은 각 메소드별로 스택 프레임(Stack Frame)을 생성한다.

- https://youtu.be/9lQsAPFQjBg: 서블릿을 잘못 구현할 경우 발생하는 문제를 보여주고 이에 대한 해결책을 제시하고 있다.

### 9.3.5 싱글톤 패턴(12번)

매번 인스턴스를 생성하지 않고 인스턴스 하나만 생성해 재사용하도록 강제할 수 있는 디자인 패턴이 싱글톤(singleton) 패턴이다.

싱글톤 패턴을 구현하려면 먼저 클래스의 기본 생성자를 `private` 접근 제어자로 구현해 클래스 외부에서 인스턴스를 생성할 수 없도록 한다.
인스턴스에 대한 생성은 `getInstance()`와 같은 `static` 메소드를 통해 가능하도록 허용한다.

```java
public class JdbcTemplate {
    private static JdbcTemplate jdbcTemplate;
    
    private JdbcTemplate() {}
   
    public static JdbcTemplate getInstance() {
        if (jdbcTemplate == null) {
            jdbcTemplate = new JdbcTemplate();
        }
        return jdbcTemplate;
    }
}
```

위와 같이 싱글톤 패턴을 구현하는 경우 여러 개의 쓰레드가 동시에 `getInstance()` 메소드를 호출하는 경우 인스턴스가 하나 이상 생성될 수 있는 문제가 있는 코드이다.

인스턴스가 하나만 생성하도록 보장하면서 간단하게 구현할 수 있는 방법은 다음과 같다.

```java
public class JdbcTemplate {
    private static JdbcTemplate jdbcTemplate = new JdbcTemplate();
    
    private JdbcTemplate() {}
   
    public static JdbcTemplate getInstance() {
        return jdbcTemplate;
    }
}
```



To be continued...
