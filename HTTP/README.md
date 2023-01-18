# HTTP (HyperText Transfer Protocol)

> [HTTP](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html) 사이트를 번역한 페이지 입니다.

## 소개

---
### 인터넷

인터넷(또는 웹)은 다음 다이어그램에 묘사된 대규모 분산 클라이언트/서버 정보 시스템입니다.

![TheWeb](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/TheWeb.png)

웹에서 동시에 여러 응용 프로그램이 실행됩니다.
예를 들어 웹 브라우징, 이메일, 파일 전송, 오디오 및 비디오 스트리밍 등이 있습니다.
클라이언트와 서버 간 정상적인 통신을 위해서는 이러한 응용 프로그램들은 HTTP, FTP, SMTP, POP 등 특정 응용 프로토콜들을 사용해야 합니다.

### HyperText Transfer Protocol (HTTP)

HTTP(Hypertext Transfer Protocol)는 인터넷(또는 웹)에서 가장 인기있는 응용 프로토콜 중 하나입니다.
- HTTP는 그림에서 보이는 것처럼 비대칭 요청-응답 클라이언트-서버 프로토콜입니다.
  HTTP 클라이언트는 HTTP 서버에게 요청 메시지를 보내며, 서버는 그에 대한 응답 메시지를 반환합니다.
  즉, HTTP는 클라이언트가 서버에서 정보를 가져오는(서버가 클라이언트로 정보를 보내는 것이 아니라) 풀 프로토콜입니다.

![HTTP](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP.png)

- HTTP는 상태 비저장 프로토콜입니다. 즉, 현재 요청은 이전 요청들이 어떤 것을 수행했는지 모릅니다.
- HTTP는 데이터 유형과 표현을 협상할 수 있도록 허용합니다. 이를 통해 데이터가 전송되는 것과 독립적으로 시스템을 구축할 수 있게 됩니다.
- RFC2616에서 인용: "HTTP(Hypertext Transfer Protocol)는 분산, 협업, 하이퍼미디어 정보 시스템을 위한 응용 계층 프로토콜 입니다.
  이것은 하이퍼텍스트를 사용하는 것 외에도 이름 서버, 분산 객체 관리 시스템 등 다양한 작업에 사용할 수 있는 일반적인, 상태less 프로토콜 입니다. 요청 메소드, 에러 코드, 헤더를 확장하는 것으로 가능합니다."

### 브라우저

브라우저를 사용해 URL을 입력하여 웹 자원을 요청할 때(예 : http://www.nowhere123.com/index.html), 브라우저는 URL을 요청 메시지로 변환하고 HTTP 서버로 전송합니다.
HTTP 서버는 요청 메시지를 해석하고 적절한 응답 메시지를 반환합니다. 이것은 요청한 자원이거나 오류 메시지 중 하나입니다.
이 프로세스는 아래에서 나타냅니다.

![HTTP_Steps](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_Steps.png)

### Uniform Resource Locator (URL)

URL(Uniform Resource Locator)는 웹상에서 자원을 고유하게 식별하는데 사용됩니다.
URL의 문법은 다음과 같습니다:
```text
protocol://hostname:port/path-and-file-name
```

URL에는 4가지 부분이 있습니다:
1. 프로토콜: 클라이언트와 서버가 사용하는 응용 계층 프로토콜, 예를 들어 HTTP, FTP, 텔넷
2. 호스트 이름: 서버의 DNS 도메인 이름(예: www.nowhere123.com) 또는 IP 주소(예: 192.128.1.2)
3. 포트: 서버가 클라이언트로부터의 요청을 대기하는 TCP 포트 번호
4. 경로와 파일 이름: 서버 문서 기본 디렉토리 하에서 요청한 자원의 이름과 위치

예를 들어, URL http://www.nowhere123.com/docs/index.html 에서 통신 프로토콜은 HTTP이며 호스트 이름은 www.nowhere123.com 입니다.
URL에 포트 번호가 지정되어 있지 않아 기본 번호인 TCP 포트 80(HTTP)를 사용합니다.
자원의 경로와 파일 이름은 "/docs/index.html" 입니다.

다른 URL 예제들은 다음과 같습니다 :
```text
ftp://www.ftp.org/docs/test.txt
mailto:user@test101.com
news:soc.culture.Singapore
telnet://www.nowhere123.com/
```

### HTTP 프로토콜

앞서 언급한 것처럼, 브라우저의 주소 상자에 URL을 입력할 때마다, 브라우저는 지정된 프로토콜에 따라 URL을 요청 메시지로 변환하고 서버로 요청 메시지를 보냅니다.

예를 들어, 브라우저는 URL http://www.nowhere123.com/docs/index.html 을 다음과 같은 요청 메시지로 변환합니다:
```text
GET /docs/index.html HTTP/1.1
Host: www.nowhere123.com
Accept: image/gif, image/jpeg, */*
Accept-Language: en-us
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
(blank line)
```

이 요청 메시지가 서버에 도착하면, 서버는 다음 중 하나의 작업을 수행할 수 있습니다:
1. 서버는 수신된 요청을 해석하고, 요청을 서버의 문서 디렉토리 하의 파일로 매핑하고, 요청된 파일을 클라이언트에게 반환합니다.
2. 서버는 수신된 요청을 해석하고, 요청을 서버에 유지되는 프로그램으로 매핑하고, 프로그램을 실행하고, 프로그램의 출력을 클라이언트에게 반환합니다.
3. 요청을 충족시킬 수 없는 경우, 서버는 에러 메시지를 반환합니다.

HTTP 응답 메시지 예제는 다음과 같습니다:
```text
HTTP/1.1 200 OK
Date: Sun, 18 Oct 2009 08:56:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Sat, 20 Nov 2004 07:16:26 GMT
ETag: "10000000565a5-2c-3e94b66c2e680"
Accept-Ranges: bytes
Content-Length: 44
Connection: close
Content-Type: text/html
X-Pad: avoid browser bug
  
<html><body><h1>It works!</h1></body></html>
```

브라우저는 응답 메시지를 수신하고, 메시지를 해석하고, 응답의 미디어 타입(Content-Type 응답 헤더와 같은)에 따라 브라우저 창에 메시지의 내용을 표시합니다.
일반적인 미디어 타입은 "text/plain", "text/html", "image/gif", "image/jpeg", "audio/mpeg", "video/mpeg", "application/msword", "application/pdf" 입니다.

HTTP 서버는 설정에 지정된 IP 주소(들)와 포트(들)로 수신되는 요청을 대기하는 것만 합니다.
요청이 도착하면 서버는 메시지 헤더를 분석하고, 설정에 지정된 규칙을 적용하고, 적절한 작업을 수행합니다.
웹마스터의 웹 서버 작업에 대한 주요 제어는 설정에 의해 이루어지며, 이는 나중의 섹션에서 자세히 설명됩니다.

To be continued...