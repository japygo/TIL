# Forward and Redirect

## Forward

웹 애플리케이션에서 Forwarding은 한 리소스(예를 들어 Servlet 또는 JSP)에서 다른 리소스로 요청을 전달하는 것입니다.
클라이언트는 이 변화를 알 수 없으며, 서버에서 내부적으로 처리됩니다.
이때, 서버에 새로운 HTTP 요청이 발생하지 않고, 클라이언트 주소창에 표시되는 URL도 변경되지 않습니다.
요청과 응답 객체가 대상 리소스로 전달되어, 대상 리소스에서 응답을 생성할 수 있습니다.

### 예시

```java
//Servlet class
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   // form data processing logic

   RequestDispatcher dispatcher = request.getRequestDispatcher("/result.jsp");
   dispatcher.forward(request, response);
}
```

사용자가 폼을 제출한 후, 폼 데이터를 처리하는 Servlet에서 결과를 다른 JSP 페이지로 Forward하는 것입니다.
이 경우, 원래의 URL은 변경되지 않고, 처리 결과만 JSP 페이지에 표시됩니다.

## Redirect

Redirect는 웹 애플리케이션에서 클라이언트의 요청을 다른 URL로 리다이렉션하는 것입니다.
이때, 클라이언트에게 리다이렉션할 URL이 전달되고, 클라이언트는 새로운 URL로 새로운 HTTP 요청을 보냅니다.
따라서 클라이언트의 주소창에 표시되는 URL이 변경됩니다.
Redirect는 웹 애플리케이션에서 정보를 공유하지 않으며, 서버와 클라이언트 사이에서 보안 관련된 요청 처리와 같은 작업을 수행할 때 유용하게 사용될 수 있습니다.

### 예시

```java
//Servlet class
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   if (!isUserLoggedIn) {
      response.sendRedirect("/login.jsp");
      return;
   }
   // other logic for handling the request
}
```

사용자가 로그인하지 않은 상태에서 보호된 페이지에 접근하려고 할 때, 로그인 페이지로 Redirect하는 것입니다.
이 경우, URL이 변경되며 로그인 페이지로의 새로운 HTTP 요청이 전송됩니다.

## Forward 와 Redirect 차이

1. URL 변경: Forward는 URL이 변경되지 않지만, Redirect는 URL이 변경됩니다.
2. HTTP 요청: Forward는 클라이언트에게 HTTP 요청이 전송되지 않지만, Redirect에서는 클라이언트에게 HTTP 요청이 전송됩니다.
3. 정보 공유: Forward에서는 현재의 요청과 응답 객체가 전달되어 두 리소스 간에 정보를 공유할 수 있지만, Redirect에서는 정보 공유가 불가능합니다.
4. 작업 내용: Forward는 원래의 리소스에서 대상 리소스로의 요청을 내부적으로 처리하는 것이고, Redirect는 요청을 완전히 새로운 리소스로 보내는 것입니다.

요청을 처리하는 웹 애플리케이션에서 Forward와 Redirect는 서로 다른 목적에 따라 적절히 사용되어야 합니다.
Forward는 두 리소스 간에 정보 공유가 필요한 경우에 유용하게 사용되지만, Redirect는 클라이언트에게 새로운 URL로 이동할 것을 요청하는 경우에 유용하게 사용됩니다.
