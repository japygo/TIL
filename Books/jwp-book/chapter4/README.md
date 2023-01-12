# 4장 HTTP 웹 서버 구현을 통해 HTTP 이해하기

## 4.1 동영상을 활용한 HTTP 웹 서버 실습
- https://youtu.be/xHQ0X_Ails4: 요구사항 1 – index.html 응답하기
- https://youtu.be/ioOGE8qTa94: 요구사항 2 – GET 방식으로 회원가입하기
- https://youtu.be/q5bvPKbc_RM: 요구사항 3 - POST 방식으로 회원가입하기
- https://youtu.be/vfCpgIJU2XU: 요구사항 4 – 302 status code 적용
- https://youtu.be/wWEW7aYS66A: 요구사항 5 – 로그인하기
- https://youtu.be/pQhCqu_nQjc: 요구사항 7 – CSS 지원하기

## 4.2 HTTP 웹 서버 구현

#### 4.2.1.1 요구사항 1 - index.html 응답하기
웹 클라이언트(대부분 웹 브라우저)는 웹 서버와 데이터를 주고 받기 위해 HTTP라는 서로 간에 약속된 규약을 따른다.
```text
POST /user/create HTTP/1.1
HOST: localhost:8080
Connection-Length: 59
Content-Type: application/x-www.form-urlencoded
Accept: */*

userId=javajigi&password=password
```
요청 데이터의 첫 번째 라인은 `요청 라인(Request Line)`, 두 번째 라인부터 빈 공백 문자열 라인까지 `요청 헤더(header)`, 빈 공백 문자열 다음부터 `본문(body)` 데이터가 전송된다.

요청 라인은 "HTTP-메소드 URI HTTP-버전"으로 구성되어 있다. HTTP 메소드는 요청의 종류를 나타낸다. URI는 클라이언트가 서버에 유일하게 식별할 수 있는 요청 자원의 경로를 의미한다.

요청 헤더는 <필드 이름>: <필드 값> 쌍으로 이루어져 있다.

이와 같이 클라이언트에서 요청을 받으면 서버는 클라이언트 요청에 대한 응답을 한다.
```text
HTTP/1.1 200 OK
Content-Type: text/html;charset=utf-8
Content-Length: 20

<h1>Hello World</h1>
```
응답 메시지의 첫 번째 라인은 `상태 라인(status)`, 두 번째 라인부터 빈 공백 문자열 라인까지 `응답 헤더(header)`, 빈 공백 문자열 다음부터 응답으로 보낼 `본문(body)` 데이터이다.

상태 라인은 "HTTP-버전 상태코드 응답구문"으로 구성되어 있다. HTTP 버전은 HTTP 요청 라인의 HTTP 버전과 같은 의미이다. 상태코드는 응답에 대한 상태를 의미하는 코드 값으로 200은 성공을 의미한다. 응답 구문은 응답 상태에 대한 설명이다.


To be continued...