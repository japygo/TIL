# 6장 서블릿/JSP를 활용해 동적인 웹 애플리케이션 개발하기

오랜 기간동안 서블릿(Servlet)과 JSP(Java Server Page)는 자바 진영에서 동적인 웹 애플리케이션을 개발하는 데 있어 표준이었다.
하지만 최근에 [플레이 프레인워크](https://www.playframework.com)와 같이 서블릿 표준을 따르지 않는 기술도 등장하고 있다.

## 6.1 서블릿/JSP로 회원관리 기능 다시 개발하기

- https://youtu.be/0VpYSAR8x28: Github에 있는 코드를 이클립스로 가져와 메이븐 빌드하기

### 6.1.1 서블릿/JSP 복습

JSP는 Java Server Page라는 이름에서도 알 수 있듰이 JSP에서 자바 구문을 그대로 사용할 수 있다.
JSP에서는 스크립틀릿(scriptlet)이라고 하는 `<% %>` 내에 자바 구문을 그대로 사용할 수 있게 되었다.
웹 애플리케이션 요구사항의 복잡도가 증가하면서 이를 극복하기 위해 등장한 기술이 JSTL(JavaServer Pages Standard Tag Library)과 EL(Expression Language)이다.

### 6.1.2 개인정보수정 실습

### 6.1.3 로그인/로그아웃 기능 실습

### 6.1.4 회원 목록 및 개인정보 수정 보안 강화 실습

### 6.1.5 중복 코드 제거

## 6.2 세션(HttpSession) 요구사항 및 실습

HTTP는 클라이언트와 서버가 연결된 후 상태를 유지할 수 없다.
따라서 HTTP를 무상태 프로토콜이라고 부른다.
상태를 유지할 필요가 있을 때 사용할 수 있는 방법이 쿠키(Cookie) 헤더를 사용하는 방법이다.

세션은 상태 값으로 유지하고 싶은 정보를 클라이언트인 브라우저에 저장하는 것이 아니라 서버에 저장한다.
서버에 저장한 후 각 클라이언트마다 고유한 아이디를 발급해 이 아이디를 `Set-Cookie` 헤더를 통해 전달한다.
세션은 HTTP의 쿠키를 기반으로 동작한다.

### 6.2.1 요구사항

- `String getId()`: 현재 세션에 할당되어 있는 고유한 세션 아이디를 반환
- `void setAttribute(String name, Object value`: 현재 세션에 `value` 인자로 전달되는 객체를 `name` 인자 이름으로 저장
- `Object getAttribute(String name)`: 현재 세션에 `name` 인자로 저장되어 있는 객체 값을 찾아 반환
- `void removeAttribute(String name)`: 현재 세션에 `name` 인자로 저장되어 있는 객체 값을 삭제
- `void invalidate()`: 현재 세션에 저장되어 있는 모든 값을 삭제

## 6.3 세션(HttpSession) 구현

### 6.3.1 고유한 아이디 생성

### 6.3.2 쿠키를 활용해 아이디 전달

### 6.3.3 모든 클라이언트의 세션 데이터에 대한 저장소 추가

### 6.3.4 클라이언트별 세션 저장소 추가

## 6.4 MVC 프레임워크 요구사항 1단계

MVC(Model, View, Controller) 패턴의 요청과 응답 흐름
1. 요청: 클라이언트 -> 서블릿(컨트롤러)
2. 요청: 서블릿(컨트롤러) -> 자바 클래스(모델)
3. 응답: 자바 클래스(모델) -> 서블릿(컨트롤러)
4. 모델: 서블릿(컨트롤러) -> JSP(뷰)
5. 응답: JSP(뷰) -> 클라이언트

### 6.4.1 요구사항

MVC 패턴을 지원하는 프레임워크를 구현한다.  
모든 클라이언트 요청은 먼저 `DispatcherServlet`이 받은 후 요청 URL에 따라 해당 컨트롤러에 작업을 위임하도록 구현한다.
`@WebServlet`으로 URL을 매핑할 때 `urlPatterns="/"`와 같이 설정하면 모든 요청이 URL이 `DispatcherServlet`으로 연결된다.  
단, CSS, 자바스크립트, 이미지와 같은 정적인 자원은 굳이 컨트롤러가 필요없다.
`core.web.filter.ResourceFilter` 서블릿 필터를 추가하여 해결할 수 있다.
 
> ### 이슈사항
> 1. 요청 -> `DispatcherServlet`
> 2. `DispatcherServlet`에서 `RequestMapping` 통해 `Controller` 반환
> 3. 해당 `Controller`에서 `View` 반환
> 4. 해당 `View`로 `forward` 또는 `sendRedirect` 응답
> 5. 응답한 `View`가 `DispatcherServlet` 다시 요청
> 6. `View`에 해당하는 `Controller`가 없기 때문에 에러 발생
>
> `DispatcherServlet`의 서블릿 매핑을 "/*"로 하면 위와 같은 에러가 발생하지만 "/"로 설정하였는데 위와 같은 에러가 발생했다.
>
> ```java
> @WebServlet(name = "dispatcher", urlPatterns = "/", loadOnStartup = 1)
> public class DispatcherServlet extends HttpServlet {}
> ```
> ### 해결방안
> 각 jsp 파일에 `<jsp:include page="/layout/header.html" />`를 이용하여 첨부되어 있어 이를 `<%@ include file="layout/header.html"%>`로 수정하니 에러가 발생하지 않았다. 
> 
> `<jsp:include />`를 사용하면 `DispatcherServlet`의 서블릿 매핑이 "/*"이 아닌 "/"이어도 첨부한 페이지가 매핑되는 것 같다.



To be continued...