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

To be continued...
