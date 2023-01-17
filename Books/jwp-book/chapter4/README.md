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

#### 4.2.1.2 요구사항 2 - GET 방식으로 회원가입하기

```text
GET /user/create?userId=javajigi&password=password&name=jaesung&email=javajigi%40slipp.net HTTP/1.1
```

요청 라인의 "GET"은 form 태그 method 속성 값이고, 요청 URI는 action 속성 값(/user/create)이다.
GET 메소드 방식으로 요청을 보낼 경우 사용자가 입력한 값을 물음표 뒤에 `매개변수명1=값1&매개변수명2=값2` 형식으로 전송한다.

"/user/create"는 요청 자원의 위치를 나타내는 `경로(path)`라 부르고, 물음표 뒤에 전달되는 매개변수를 `쿼리 스트링(query string)`이라고 부른다.

#### 4.2.1.3 요구사항 3 - POST 방식으로 회원가입하기

```text
POST /user/create HTTP/1.1
```

쿼리 스트링은 HTTP 요청의 본문(body)을 통해 전달된다.
POST 방식으로 데이터를 전달하면 헤더에 본문 데이터에 대한 길이가 Content-Length라는 필드 이름으로 전달된다.

#### 4.2.1.4 요구사항 4 - 302 status code 적용

회원가입 후 새로고침을 하면 이전과 똑같은 회원가입 요청이 발생한다.
이 같은 현상이 발생하는 이유는 브라우저가 이전 요청 정보를 유지하고 있기 때문이다.

이 문제를 해결하는 방법은 회원가입을 처리하는 /user/create 요청과 첫 화면(/index.html)을 보여주는 요청을 분리한 후 HTTP의 302 상태코드를 활용해 해결할 수 있다.

```text
HTTP/1.1 302 Found
Location: /index.html
```

위와 같이 응답을 보내면 클라이언트는 첫 라인의 상태 코드를 확인한 후 302라면 Location의 값을 읽어 서버에 재요청을 보내게 된다.

302 상태 코드를 활용해 페이지를 이동할 경우 요청과 응답이 한 번이 아니라 두 번 발생한다. 302 상태 코드를 활용한 페이지 이동 방식은 많은 라이브러리와 프레임워크에서 리다이렉트 이동 방식으로 알려져 있다.

- 2xx : 성공. 클라이언트가 요청한 동작을 수신하여 이해했고 승낙했으며 성공적으로 처리.
- 3xx : 리다이렉션. 클라이언트는 요청을 마치기 위해 추가 동작이 필요함.
- 4xx : 요청 오류. 클라이언트에 오류가 있음.
- 5xx : 서버 오류. 서버가 유효한 요청을 명백하게 수행하지 못했음.

#### 4.2.1.5 요구사항 5 - 로그인하기

HTTP는 요청을 보내고 응답을 받으면 HTTP 1.1부터는 한번 맺은 연결을 재사용한다.
연결은 재사용하지만 각 요청 간의 상태 데이터를 공유할 수는 없는 무상태 프로토콜의 특성을 가진다.
HTTP가 무상태 프로토콜이기 때문에 서버는 클라이언트가 누구인지 식별할 수 있는 방법이 없다는 문제가 발생한다.

HTTP는 로그인과 같이 클라이언트의 행위를 기억하기 위한 목적으로 지원하는 것이 `쿠키(Cookie)`이다.
클라이언트는 응답 헤더에 Set-Cookie 가 존재할 경우 Set-Cookie의 값을 읽어 서버에 보내는 요청 헤더의 Cookie 헤더 값으로 다시 전송한다.
즉, HTTP는 각 요청 간에 데이터를 공유할 방법이 없기 때문에 헤더를 통해 공유할 데이터를 매번 다시 전송하는 방식으로 데이터를 공유한다.

####  4.2.1.6 요구사항 6 - 사용자 목록 출력

로그인 상태 유무와 같이 각 요청 간에 상태 정보를 공유하기 위한 방법으로 쿠키를 사용한다.
서버가 전달하는 쿠키 정보는 클라이언트에 저장해 관리하기 때문에 보안 이슈가 있다.
이 같은 단점을 보완하기 위해 세션이 등장했다. 세션 또한 쿠키를 기반으로 하는 것은 같다.
단, 좀 더 보안을 강화하기 위한 방법으로 상태 데이터를 서버에 저장한다는 것만 다르다.

#### 4.2.1.7 요구사항 7 - CSS 지원하기

브라우저는 응답을 받은 후 Content-Type 헤더 값을 통해 응답 본문에 포함되어 있는 컨텐츠가 어떤 컨텐츠인지를 판단한다.

각 요청과 응답 헤더는 각 요청과 응답이 포함하고 있는 본문 컨텐츠에 대한 정보를 제공하고 있다. 이와 같이 데이터에 대한 정보를 포함하고 있는 헤더 정보들을 `메타데이터`라고 부른다.

- https://youtu.be/VeTjDYl7UVs: 이클립스 또는 터미널에서 브랜치 변경 방법

## 4.3 추가 학습 자료

### 4.3.1 HTTP

- [HTTP (HyperText Transfer Protocol)](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html)
- [프로가 되기 위한 웹 기술 입문](http://www.yes24.com/Product/Goods/6721651)(고모리 유스케 저/김정환 역, 위키북스/2012)
- [HTTP & Network : 그림으로 배우는 책으로 학습](http://www.yes24.com/Product/Goods/15894097)(우에노 센 저/이병억 역, 영진닷컴/2015)
- [HTTP 완벽 가이드](http://www.yes24.com/Product/Goods/15381085)(데이빗 고울리,브라이언 토티,마조리 세이어,세일루 레디,안슈 아가왈 공저/이응준,정상일 공역, 인사이트(insight)/2014)

### 4.3.2 네트워크

- [성공과 실패를 결정하는 1%의 네트워크 원리](http://www.yes24.com/Product/Goods/90640081)(Tsutomu Tone 저/이도희 역/이중호 감역, 성안당/2015)