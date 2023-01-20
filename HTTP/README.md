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

### HTTP over TCP/IP

HTTP는 클라이언트-서버 응용 계층 프로토콜입니다. 그것은 일반적으로 TCP/IP 연결 위에서 실행됩니다. (HTTP는 TCP/IP 상에서만 실행될 필요는 없습니다. 그것은 단지 신뢰성 있는 전송을 가정합니다. 이러한 보장을 제공하는 전송 프로토콜은 사용할 수 있습니다.)

![HTTP_OverTCPIP](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_OverTCPIP.png)

TCP/IP(Transmission Control Protocol/Internet Protocol)는 네트워크를 통해 컴퓨터 간에 통신하기 위한 전송계층과 네트워크 계층 프로토콜의 집합입니다.

IP(Internet Protocol)는 네트워크 계층 프로토콜로, 네트워크 주소와 라우팅에 관련됩니다.
IP 네트워크에서는 각 머신에 고유한 IP 주소(예: 165.1.2.3)가 할당되며, IP 소프트웨어는 소스 IP에서 대상 IP로 메시지를 라우팅하는 일을 담당합니다.
IPv4(IP 버전 4)에서 IP 주소는 4바이트로 구성되며, 각 바이트는 0~255의 범위를 가지며, 점으로 구분되며, 이를 쿼드-도트 형식이라고 합니다.
이 네이밍 스키마는 네트워크상에서 4G 주소까지 지원합니다.
최신 IPv6(IP 버전 6)은 더 많은 주소를 지원합니다.
대부분의 사람들에게 숫자를 기억하기 어렵기 때문에, www.nowhere123.com 같은 영어 같은 도메인 이름을 사용합니다.
DNS(도메인 이름 서비스)는 도메인 이름을 IP 주소로 변환합니다(분산된 조회 테이블을 통해).
특수 IP 주소 127.0.0.1은 항상 자신의 머신을 가리킵니다.
그 도메인 이름은 "localhost"이며, 로컬 루프백 테스팅에 사용할 수 있습니다.

TCP(Transmission Control Protocol)는 전송 계층 프로토콜로, 두 머신 간의 연결을 설정하는 일을 담당합니다.
TCP는 2가지 프로토콜로 구성되어 있습니다: TCP와 UDP(User Datagram Package).
TCP는 신뢰성이 있으며, 각 패킷에는 시퀀스 번호가 있으며, 응답을 기대합니다.
수신자가 받지 못한 패킷은 재전송됩니다. TCP는 패킷 전달을 보장합니다.
UDP는 패킷 전달을 보장하지 않으므로 신뢰성이 없습니다.
그러나 UDP는 네트워크 오버헤드가 적어서 비디오와 오디오 스트리밍과 같은 응용 프로그램에서는 신뢰성이 중요하지 않은 경우에 사용될 수 있습니다.

TCP는 IP 머신 내에서 응용 프로그램을 멀티플렉싱합니다.
각 IP 머신당 TCP는 (65536개의 포트나 소켓) 지원하며, 0번 포트부터 65535번 포트까지 있습니다.
응용 프로그램, 예를 들어 HTTP나 FTP,은 특정 포트 번호에서 수신 요청을 기다립니다.
포트 0부터 1023번은 인기있는 프로토콜에 사전 할당되어 있습니다.
예를들면 HTTP가 80번, FTP가 21번, Telnet이 23번, SMTP가 25번, NNTP가 119번, DNS가 53번입니다.
포트 1024번 이상은 사용자에게 사용 가능합니다.

포트 80번이 HTTP에 할당되어 있지만, 기본 HTTP 포트 번호라는 것은 다른 사용자가 지정한 포트 번호 (1024-65535)를 사용하여 HTTP 서버를 실행하는 것을 금지하지 않습니다.
예를 들어 8000번, 8080번은 테스트 서버에서 사용할 수 있습니다.
같은 머신에서 다른 포트 번호로 여러 HTTP 서버를 실행할 수도 있습니다.
클라이언트가 URL을 요청할 때 포트 번호를 명시적으로 지정하지 않는다면, 예를 들어 http://www.nowhere123.com/docs/index.html, 브라우저는 호스트 www.nowhere123.com의 기본 포트 번호 80에 연결합니다.
하지만 서버가 8000번 포트에서 대기하고 있지 않고 기본 포트 80에서 대기하고 있다면 URL에 포트 번호를 명시적으로 지정해야 합니다.
예를들면 http://www.nowhere123.com:8000/docs/index.html

간단히 말하면, TCP/IP를 통해 통신하려면 (a) IP 주소 또는 호스트 이름 (b) 포트 번호를 알아야 합니다.

### HTTP 사양

HTTP 사양은 [W3C (World-wide Web Consortium)](https://www.w3.org/)에서 관리하며 http://www.w3.org/standards/techs/http 에서 사용 가능합니다.
현재 HTTP/1.0과 HTTP/1.1이 두 가지 버전이 있습니다.
원래 버전인 HTTP/0.9 (1991)는 Tim Berners-Lee가 작성한 Internet을 건너 원시 데이터를 전송하는 간단한 프로토콜입니다.
HTTP/1.0 (1996)는 MIME 같은 메시지를 허용하여 프로토콜을 개선했습니다.(RFC 1945에 정의됨)
HTTP/1.0은 프록시, 캐싱, 지속적인 연결, 가상 호스트, 및 범위 다운로드 문제를 해결하지 않습니다.
이러한 기능은 HTTP/1.1 (1999)에서 제공되었습니다.(RFC 2616에 정의됨)

## Apache HTTP 서버 또는 Apache Tomcat 서버

---

HTTP 프로토콜을 연구하려면 HTTP 서버 (예 : Apache HTTP 서버 또는 Apache Tomcat 서버)가 필요합니다.

Apache HTTP 서버는 산업 규모의 생산 서버로서 인기가 많습니다.
Apache Software Foundation (ASF)에서 생산하며 www.apache.org 에서 제공됩니다.
ASF는 오픈 소스 소프트웨어 재단입니다. 즉, Apache HTTP 서버는 소스 코드와 함께 무료로 제공됩니다.

첫 번째 HTTP 서버는 Tim Berners-Lee가 작성하였으며 제네바의 유럽 원자력 연구 센터(CERN)에서 HTML을 발명했습니다.
Apache는 1995년 초에 NCSA (미국 과학 계산 응용 센터)의 "httpd 1.3" 서버를 기반으로 개발되었습니다.
Apache는 이전 NCSA httpd 웹 서버의 일부 코드와 패치를 포함하기 때문에 이름을 얻었다고 생각되거나, 미국 원주민 인디언 전쟁에서 유명한 인디언 족의 이름에서 유래한 것으로 추측됩니다.

[Apache How-to](https://www3.ntu.edu.sg/home/ehchua/programming/howto/Apache_HowToInstall.html)를 읽어보면 Apache HTTP 서버를 설치하고 구성하는 방법을 알 수 있습니다.
또는 [Tomcat How-to](https://www3.ntu.edu.sg/home/ehchua/programming/howto/Tomcat_HowTo.html)를 읽어보면 Apache Tomcat 서버를 설치하고 시작하는 방법을 알 수 있습니다.

## HTTP 요청 및 응답 메시지

---

HTTP 클라이언트와 서버는 텍스트 메시지를 전송하여 통신합니다.
클라이언트는 서버에 요청 메시지를 보냅니다.
그리고 서버는 응답 메시지를 반환합니다.

HTTP 메시지는 메시지 헤더와 선택적인 메시지 바디로 구성되어 있으며, 공백 라인으로 구분됩니다.
아래와 같이 구성됩니다.

![HTTP_MessageFormat](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_MessageFormat.png)

### HTTP 요청 메시지

HTTP 요청 메시지의 형식은 다음과 같습니다:

![HTTP_RequestMessage](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_RequestMessage.png)

#### 요청 라인

헤더의 첫번째 줄은 요청 라인이라고 불리며, 선택적 요청 헤더가 따라옵니다.

요청 라인은 다음과 같은 구문을 가집니다:

```text
request-method-name request-URI HTTP-version
```

- 요청 메소드 이름: HTTP 프로토콜은 GET, POST, HEAD, OPTIONS와 같은 요청 메소드 세트를 정의합니다.
  클라이언트는 이들 중 하나의 메소드를 사용하여 서버로 요청을 보낼 수 있습니다.
- 요청 URI: 요청된 리소스를 지정합니다.
- HTTP 버전: 현재는 HTTP/1.0과 HTTP/1.1이 사용되고 있습니다.

요청 라인의 예는 다음과 같습니다:

```text
GET /test.html HTTP/1.1
HEAD /query.html HTTP/1.0
POST /index.html HTTP/1.1
```

#### 요청 헤더

요청 헤더는 이름:값 쌍의 형식으로 작성됩니다.
하나의 이름에 여러 값을 쉼표로 구분하여 지정할 수 있습니다.

```text
request-header-name: request-header-value1, request-header-value2, ...
```

요청 헤더 예는 다음과 같습니다.

```text
Host: www.xyz.com
Connection: Keep-Alive
Accept: image/gif, image/jpeg, */*
Accept-Language: us-en, fr, cn
```

#### 예시

다음은 샘플 HTTP 요청 메시지를 보여줍니다:

![HTTP_RequestMessageExample](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_RequestMessageExample.png)

### HTTP 응답 메시지

HTTP 응답 메시지의 형식은 다음과 같습니다:

![HTTP_ResponseMessage](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_ResponseMessage.png)

#### 상태 라인

첫번째 줄은 상태 라인이라고 불리며, 선택적 응답 헤더가 따라옵니다.

상태 라인은 다음 구문을 가지고 있습니다.

```text
HTTP-version status-code reason-phrase
```

- HTTP 버전: 이 세션에서 사용된 HTTP 버전. HTTP/1.0 또는 HTTP/1.1 중 하나.
- 상태 코드: 서버에서 생성한 요청 결과를 반영하는 3자리 숫자.
- 이유 구문: 상태 코드에 대한 간단한 설명.
- 일반적인 상태 코드와 이유 구문은 "200 OK", "404 Not Found", "403 Forbidden", "500 Internal Server Error" 입니다.

상태 라인의 예는 다음과 같습니다.

```text
HTTP/1.1 200 OK
HTTP/1.0 404 Not Found
HTTP/1.1 403 Forbidden
```

#### 응답 헤더

응답 헤더는 이름:값 쌍의 형식입니다.

```text
response-header-name: response-header-value1, response-header-value2, ...
```

응답 헤더의 예는 다음과 같습니다.

```text
Content-Type: text/html
Content-Length: 35
Connection: Keep-Alive
Keep-Alive: timeout=15, max=100
```

응답 메시지 바디는 요청한 리소스 데이터를 포함합니다.

#### 예시

아래는 샘플 응답 메시지를 보여줍니다.

![HTTP_ResponseMessageExample](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_ResponseMessageExample.png)

## HTTP 요청 메소드

---

HTTP 프로토콜은 요청 메소드의 세트를 정의합니다.
클라이언트는 이러한 요청 메소드 중 하나를 사용하여 HTTP 서버에 요청 메시지를 보낼 수 있습니다.
이 메소드들은 다음과 같습니다:

- GET: 클라이언트는 GET 요청을 사용하여 서버에서 웹 리소스를 가져올 수 있습니다.
- HEAD: 클라이언트는 HEAD 요청을 사용하여 GET 요청이 가져온 헤더를 가져올 수 있습니다.
  헤더에는 데이터의 최종 수정 날짜가 포함되어 있기 때문에 이를 사용하여 로컬 캐시 복사본과 비교할 수 있습니다.
- POST: 웹 서버로 데이터를 전송하는 데 사용됩니다.
- PUT: 서버에 데이터를 저장하도록 요청합니다.
- DELETE: 서버에게 데이터를 삭제하라고 요청합니다.
- TRACE: 서버에게 자신이 취하는 조치의 진단 추적을 반환하라는 요청을 합니다.
- OPTIONS: 서버에게 지원하는 요청 메소드의 목록을 반환하라는 요청을 합니다.
- CONNECT: 프록시가 다른 호스트에 연결하고 그냥 컨텐츠를 답으로 보내도록 요청하는 메소드입니다.
  이는 대개 프록시를 통한 SSL 연결을 만드는 데 사용됩니다.
- 다른 확장 메소드들.

## GET 요청 메소드

---

GET 요청 메소드는 가장 일반적인 HTTP 요청 메소드입니다.
클라이언트는 GET 요청 메소드를 사용하여 HTTP 서버에서 리소스를 요청(혹은 "가져오기") 할 수 있습니다.
GET 요청 메시지는 다음 구문을 가집니다:

```text
GET request-URI HTTP-version
(optional request headers)
(blank line)
(optional request body)
```

- GET 키워드는 대소문자를 구분하며, 반드시 대문자로 작성해야 합니다.
- 요청 URI : 요청한 리소스의 경로를 지정하며, 문서 기본 디렉토리의 루트 "/"에서 시작해야 합니다.
- HTTP 버전 : HTTP/1.0 또는 HTTP/1.1 중 하나입니다.
  이 클라이언트는 현재 세션에 사용할 프로토콜을 협상합니다.
  예를 들어, 클라이언트가 HTTP/1.1을 요청할 수 있습니다.
  서버가 HTTP/1.1을 지원하지 않는 경우, 응답에서 HTTP/1.0을 사용하도록 클라이언트에 알려줄 수 있습니다.
- 클라이언트는 선택적 요청 헤더 (예: Accept, Accept-Language 등)를 사용하여 서버와 협상하고 서버가 선호하는 콘텐츠 (예: 클라이언트가 선호하는 언어로)를 전달하도록 요청합니다.
- GET 요청 메시지는 선택적 요청 바디를 가지며 쿼리 문자열 (나중에 설명할)을 포함합니다.

### HTTP 요청 테스트

HTTP 요청을 테스트하는 방법은 많습니다.
"telnet" 또는 "hyperterm"과 같은 유틸리티 프로그램을 사용할 수 있거나, c:\windows 하위에서 "telnet.exe" 또는 "hypertrm.exe"를 검색하거나, 자체 네트워크 프로그램을 작성하여 원시 요청 메시지를 HTTP 서버로 전송하여 다양한 HTTP 요청을 테스트 할 수 있습니다.

#### Telnet

"Telnet"은 매우 유용한 네트워킹 유틸리티입니다.
Telnet을 사용하면 서버와 TCP 연결을 설정할 수 있으며, 원시 HTTP 요청을 보낼 수 있습니다.
예를 들어, 로컬호스트 (IP 주소 127.0.0.1)에서 8000번 포트에서 HTTP 서버를 시작했다고 가정합니다.

```text
> telnet
telnet> help
... telnet help menu ...
telnet> open 127.0.0.1 8000
Connecting To 127.0.0.1...
GET /index.html HTTP/1.0
(Hit enter twice to send the terminating blank line ...)
... HTTP response message ...
```

Telnet은 문자 기반 프로토콜입니다.
Telnet 클라이언트에서 입력한 각 문자는 즉시 서버로 전송됩니다.
따라서 원시 명령을 입력할 때 오타를 만들 수 없으며, 삭제와 백스페이스는 서버로 전송됩니다.
입력한 문자를 보려면 "local echo" 옵션을 사용해야 할 수도 있습니다.
Telnet 매뉴얼(Windows의 도움말 검색)을 참조하여 Telnet 사용법을 자세히 알아보세요.

#### 네트워크 프로그램

HTTP 서버로 원시 HTTP 요청을 보내는 것도 가능합니다.
네트워크 프로그램은 먼저 서버와 TCP/IP 연결을 설정해야 합니다.
TCP 연결이 설정되면 원시 요청을 전송할 수 있습니다.

예를 들어, Java로 작성된 네트워크 프로그램 예제는 다음과 같습니다. (HTTP 서버가 로컬호스트 (IP 주소 127.0.0.1)에서 8000번 포트에서 실행된다고 가정합니다):

```java
import java.net.*;
import java.io.*;
   
public class HttpClient {
   public static void main(String[] args) throws IOException {
      // The host and port to be connected.
      String host = "127.0.0.1";
      int port = 8000;
      // Create a TCP socket and connect to the host:port.
      Socket socket = new Socket(host, port);
      // Create the input and output streams for the network socket.
      BufferedReader in
         = new BufferedReader(
              new InputStreamReader(socket.getInputStream()));
      PrintWriter out
         = new PrintWriter(socket.getOutputStream(), true);
      // Send request to the HTTP server.
      out.println("GET /index.html HTTP/1.0");
      out.println();   // blank line separating header & body
      out.flush();
      // Read the response and display on console.
      String line;
      // readLine() returns null if server close the network socket.
      while((line = in.readLine()) != null) {
         System.out.println(line);
      }
      // Close the I/O streams.
      in.close();
      out.close();
   }
}
```

### HTTP/1.0 GET 요청

다음은 HTTP/1.0 GET 요청의 응답(telnet 또는 자체 네트워크 프로그램을 통해 발신한 것 - HTTP 서버를 시작했다고 가정) 입니다.

```text
GET /index.html HTTP/1.0
(enter twice to create a blank line)
```

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
   
Connection to host lost.
```

이 예제에서는 클라이언트는 "/index.html" 이라는 이름의 문서를 요청하는 GET 요청을 보내고 HTTP/1.0 프로토콜을 사용하도록 협상합니다.
요청 헤더 뒤에는 빈 줄이 필요합니다.
이 요청 메시지는 바디를 포함하지 않습니다.

서버는 요청 메시지를 수신하여 요청 URI를 서버의 문서 디렉토리에 있는 문서로 매핑합니다.
요청된 문서가 사용 가능한 경우 서버는 "200 OK"라는 응답 상태 코드와 함께 문서를 반환합니다.
응답 헤더는 반환된 문서에 대한 필수 설명을 제공하며, 예를 들어 최종 수정 날짜(Last-Modified), MIME 유형(Content-Type), 문서 길이(Content-Length)와 같은 정보를 포함합니다.
응답 바디는 요청된 문서를 포함합니다.
브라우저는 미디어 유형(예: Plain-text, HTML, JPEG, GIF 등)과 응답 헤더에서 얻은 기타 정보에 따라 문서의 형식을 지정하고 표시합니다.

참고:
- 요청 메소드 이름 "GET"은 대소문자를 구분하며 반드시 대문자로 입력해야 합니다.
- 요청 메소드 이름이 잘못 입력되면 서버는 "501 Method Not Implemented"라는 오류 메시지를 반환합니다.
- 요청 메소드 이름이 허용되지 않으면 서버는 "405 Method Not Allowed"라는 오류 메시지를 반환합니다.
  예를 들어 DELETE는 유효한 메소드 이름이지만 서버에서 허용되지 않을 수 있습니다.
- 요청 URI가 존재하지 않으면 서버는 "404 Not Found"라는 오류 메시지를 반환합니다.
  요청 URI는 문서 기본 디렉토리의 루트 "/"에서 시작하도록 제대로 작성해야 합니다.
  그렇지 않으면 서버는 "400 Bad Request"라는 오류 메시지를 반환합니다.
- HTTP 버전이 누락되거나 잘못되면 서버는 "400 Bad Request"라는 오류 메시지를 반환합니다.
- HTTP/1.0에서는 기본적으로 서버는 응답을 전송 후 TCP 연결을 끊습니다.
  Telnet을 사용해 서버에 연결하면 응답 바디를 받은 즉시 "Connection to host lost"라는 메시지가 표시됩니다.
  "Connection: Keep-Alive"라는 선택적 요청 헤더를 사용하여 지속적인(또는 Keep-Alive) 연결을 요청하여 동일한 TCP 연결을 통해 다른 요청을 전송하여 네트워크 효율을 높일 수 있습니다.
  반면에 HTTP/1.1은 기본적으로 Keep-Alive 연결을 사용합니다.

### 응답 상태 코드

응답 메시지의 첫 번째 줄(즉, 상태 행)에는 서버가 요청 결과를 나타내는 응답 상태 코드가 포함됩니다.

응답 상태 코드는 3 자리 숫자입니다:
- 1xx (정보) : 요청이 수신되었으며 서버는 처리를 계속 진행합니다.
- 2xx (성공) : 요청이 성공적으로 수신, 이해, 수락 및 서비스되었습니다.
- 3xx (리다이렉션) : 요청을 완료하기 위해 더 이상의 조치가 필요합니다.
- 4xx (클라이언트 오류) : 요청에 문법 오류가 포함되어 이해할 수 없습니다.
- 5xx (서버 오류) : 서버가 유효한 요청을 수행하지 못했습니다.

일반적으로 만나는 상태 코드:
- 100 Continue: 서버가 요청을 받았으며 응답을 준비 중입니다.
- 200 OK: 요청이 완료되었습니다.
- 301 Move Permanently: 요청한 리소스가 새 위치로 영구적으로 이동되었습니다.
  새 위치의 URL은 응답 헤더인 Location에 제공됩니다.
  클라이언트는 새 위치로 새로운 요청을 해야합니다.
  응용 프로그램은 이 새로운 위치의 모든 참조를 업데이트해야합니다.
- 302 Found & Redirect (or Move Temporarily):
- 304 Not Modified:
- 400 Bad Request:
- 401 Authentication Required:
- 403 Forbidden:
- 404 Not Found:
- 405 Method Not Allowed:
- 408 Request Timeout:
- 414 Request URI too Large:
- 500 Internal Server Error:
- 501 Method Not Implemented:
- 502 Bad Gateway:
- 503 Service Unavailable:
- 504 Gateway Timeout:

To be continued...
