## <div align="center">HTTP/HTTPS</div>
<div align="center">마지막으로 공부한 날짜 : 2022.12.07</div>

</br></br>
## HTTP란?

- **H**yper**T**ext **T**ransfer **P**rotocol의 약자
- 웹상에서 클라이언트와 서버간의 통신을 위한 프로토콜
- 비연결성, 무상태 특징을 가짐

</br>

### **비연결성 ( Connection-less )⭐**

- 우편 시스템과 유사 - 연결 설정 및 연결 종료 불필요
- 패킷은 동일한 경로를 따르지 않음
- ex) UDP

### 연결 지향 ( **Connection-oriented** )

- 전화 시스템과 유사 - 연결 설정 및 연결 종료 필요
- 패킷은 동일한 경로를 따름
- ex) TCP

</br>

### **무상태 ( Stateless )⭐**

- 클라이언트가 현재 상태에 따라 서버에 요청을 보내고 서버가 응답하는 네트워크 프로토콜 유형
- 서버가 서버 정보 또는 세션 세부 정보를 유지할 필요가 없음
- 서버 설계를 단순화 ⇒ 아키텍처 확장이 비교적 쉬움
- 복구해야 하는 상태가 없고, 장애가 발생한 서버가 충돌 후 다시 시작될 수 있기 때문에 충돌 시 더 잘 작동 O
- ex) HTTP, UDP, DNS

### **상태 ( Stateful )**

- 클라이언트가 서버에 요청을 전송하면 어떤 종류의 응답을 기대하고, 응답을 받지 못하면 요청을 다시 보냄
- 상태 저장 프로토콜을 사용하려면 서버가 상태 및 세션 정보를 저장해야 함
- 서버 설계를 복잡하고 무겁게 함 ⇒ 아키텍처 확장이 비교적 어렵고 복잡함
- 내부 상태의 상태 및 세션 세부 정보를 유지해야 하므로 상태 프로토콜은 충돌 시 잘 작동 X
- ex) FTP, TCP, Telnet

</br></br>
# 📃 HTTP 버전 별 특징
[한눈에 보기](https://www.notion.so/dece-st/80016db75d114a24ab593b328ed466a9?v=27f7428cae354801ba4cbb3d60b4b69c&pvs=4)
## HTTP v0.9 
- *summary : **only GET**, **only HTML***

초기에는 버전 번호가 없었고 차후에 구별하기 위해 0.9를 붙임

```markdown
$> telnet google.com 80

Connected to 74.125.xxx.xxx

GET /about/

(hypertext response)
(connection closed)
```

### 특징
- **The One-Line Protocol**
- 클라이언트-서버, 요청-응답 프로토콜
- TCP/IP 링크를 통해 실행되는 ASCII 프로토콜
- 하이퍼텍스트 문서(HTML)를 전송하도록 설계
- 새로운 request마다 새로운 connection을 맺음

</br></br>
## HTTP v1.0 [RFC 1945]
- *summary : **Metadata***

```markdown
$> telnet website.org 80

Connected to xxx.xxx.xxx.xxx

GET /rfc/rfc1945.txt HTTP/1.0 1
User-Agent: CERN-LineMode/2.15 libwww/2.17b3
Accept: */*

HTTP/1.0 200 OK 2
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 01 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 1 May 1996 12:45:26 GMT
Server: Apache 0.84

(plain-text response)
(connection closed)
```

### 특징
- 요청은 여러 개의 새 줄로 구분된 헤더 필드로 구성될 수 있음
- 응답 개체에 응답 상태 줄 앞에 접두사가 붙음
- 응답 개체에는 고유한 새 줄 구분 헤더 필드 집합이 있음
- 응답 개체는 “HTML 파일, 일반 텍스트 파일, 이미지 또는 기타 콘텐츠 유형”이 가능해짐
- 새로운 request마다 새로운 connection을 맺음

</br></br>
## HTTP v1.1 [RFC 2068]
- *summary : **Persistent Connection**, **Pipelining**, **HOL Blocking***

```markdown
$> telnet website.org 80
Connected to xxx.xxx.xxx.xxx

GET /index.html HTTP/1.1 1
Host: website.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4)... (snip)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip,deflate,sdch
Accept-Language: en-US,en;q=0.8
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
Cookie: __qca=P0-800083390... (snip)

HTTP/1.1 200 OK 2
Server: nginx/1.0.11
Connection: keep-alive
Content-Type: text/html; charset=utf-8
Via: HTTP/1.1 GWA
Date: Wed, 25 Jul 2012 20:23:35 GMT
Expires: Wed, 25 Jul 2012 20:23:35 GMT
Cache-Control: max-age=0, no-cache
Transfer-Encoding: chunked

100 3
<!doctype html>
(snip)

100
(snip)

0 4

GET /favicon.ico HTTP/1.1 5
Host: www.website.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4)... (snip)
Accept: */*
Referer: http://website.org/
Connection: close 6
Accept-Encoding: gzip,deflate,sdch
Accept-Language: en-US,en;q=0.8
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
Cookie: __qca=P0-800083390... (snip)

HTTP/1.1 200 OK 7
Server: nginx/1.0.11
Content-Type: image/x-icon
Content-Length: 3638
Connection: close
Last-Modified: Thu, 19 Jul 2012 17:51:44 GMT
Cache-Control: max-age=315360000
Accept-Ranges: bytes
Via: HTTP/1.1 GWA
Date: Sat, 21 Jul 2012 21:35:22 GMT
Expires: Thu, 31 Dec 2037 23:55:55 GMT
Etag: W/PSA-GAu26oXbDi

(icon data)
(connection closed)
```

### 특징

- **지정한 시간동안 연결 유지 - Persistent Connection**
- **하나의 연결에서 응답을 대기하지 않고 여러 요청을 연속적으로 보낼 수 있음 - Pipelining**

### 단점

- HOL Blocking : 먼저 떠난 요청에 답이 없으면 무한 대기 상태
- Header 구조의 중복

</br></br>
## HTTP v2 [RFC 7540]
- *summary : **Binary Framing**, **Stream Prioritization**, **Server Push**, **Header Compression**, **Multiplexing***

기존에서  “확장 + 성능향상”을 이루어낸 버전 

### 특징

1. **메시지 전송 방식의 변화 - Binary Framing**
2. **중요한 데이터 우선 처리 - Stream Prioritization**
3. **클라이언트에서 요청을 하지 않아도 리소스를 전송하는 서버의 기능 - Server Push**
4. **여러 정보로 너무 긴 헤드를 압축 - Header Compression**
Static Dynamic Table로 중복을 검출
중복 o → 인덱스만, 중복x → 허프만 인코딩(문자열을 문자단위로 쪼개 빈도수에 따라 비트수를 분배)
기존 길이의 85% 감소
5. **여러개의 스트림을 사용해 송수신 함으로써 순서상관없이 처리 가능 - Multiplexing**

</br></br>
## HTTPS [RFC 2818]

- **H**yper**T**ext **T**ransfer **P**rotocol **S**ecure의 약자
- 애플리케이션 계층과 전송 계층 사이에 신뢰계층인 SSL/TLS 계층을 넣어 신뢰할 수 있는 HTTP 요청
- SSL/TLS와 함께 HTTP/2를 HTTPS로 사용

|  | HTTP | HTTPS |
| --- | --- | --- |
| 의미 | HyperText Transfer Protocol | HyperText Transfer Protocol Secure |
| 기본 프로토콜 | HTTP/1과 HTTP/2는 TCP/IP를 사용합니다. HTTP/3은 QUIC 프로토콜을 사용합니다. | HTTP 요청 및 응답을 추가로 암호화하기 위해 SSL/TLS와 함께 HTTP/2 사용 |
| 포트 | 기본 포트 80 | 기본 포트 443 |
| 용도 | 이전 텍스트 기반 웹 사이트 | 모든 최신 웹 사이트 |
| 보안 | 추가 보안 기능 없음 | 퍼블릭 키 암호화에 SSL 인증서 사용 |
| 이점 | 인터넷을 통한 통신 지원 | 웹 사이트에 대한 권위, 신뢰성 및 검색 엔진 순위 개선 |


</br></br>
## HTTP v3 [QUIC : RFC 9000]

- **UDP 기반의 QUIC 프로토콜** : tcp는 신뢰성을 보장하기 위한 구조가 많음(지연을 줄이기 힘든 구조)
- HTTP/2를 한 단계 더 발전시킨 것
- HTTP/3의 목표는 실시간 스트리밍 및 기타 최신 데이터 전송 요구 사항을 보다 효율적으로 지원하는 것임

TCP 및 TLS를 통해 기존 HTTP/2를 사용하는 경우 TCP는 클라이언트와 서버 간에 세션을 설정하기 위해 핸드셰이크가 필요하고 TLS는 세션이 보호되도록 자체 핸드셰이크도 필요한데,  각 핸드셰이크는 클라이언트와 서버 사이를 완전히 왕복해야 하며, 클라이언트와 서버가 멀리 떨어져 있을 때 네트워크 측면에서 시간이 오래 소요됨

 하지만 QUIC은 보안 세션을 설정하기 위해 한 번의 핸드셰이크만 필요
 
<img src="https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2022/08/06/2022-cloudfront-quic-1.jpg">

</br></br>
# 🤨 관련 질문
<details>
<summary>HTTP와 HTTPS의 차이점은 무엇인가요?</summary>
<div markdown="1">
HTTP(Hypertext Transfer Protocol)는 클라이언트와 서버 간 통신을 위한 통신 규칙 세트 또는 프로토콜입니다. 사용자가 웹 사이트를 방문하면 사용자 브라우저가 웹 서버에 HTTP 요청을 전송하고 웹 서버는 HTTP 응답으로 응답합니다. 웹 서버와 사용자 브라우저는 데이터를 일반 텍스트로 교환합니다. 간단히 말해 HTTP 프로토콜은 네트워크 통신을 작동하게 하는 기본 기술입니다. 이름에서 알 수 있듯이 HTTPS(Hypertext Transfer Protocol Secure)는 HTTP의 확장 버전 또는 더 안전한 버전입니다. HTTPS에서는 브라우저와 서버가 데이터를 전송하기 전에 안전하고 암호화된 연결을 설정합니다.
    
— HTTPS 페이지의 표 참고 —
</div>
</details>

<details>
<summary>HTTPS 프로토콜은 어떻게 작동하나요?</summary>
<div markdown="1">
HTTP는 암호화되지 않은 데이터를 전송합니다. 즉, 브라우저에서 전송된 정보를 제3자가 가로채고 읽을 수 있습니다. 이는 이상적인 프로세스가 아니었기 때문에, 통신에 또 다른 보안 계층을 추가하기 위해 HTTPS로 확장되었습니다. HTTPS는 HTTP 요청 및 응답을 SSL 및 TLS 기술에 결합합니다.
    
HTTPS 웹 사이트는 독립된 인증 기관(CA)에서 SSL/TLS 인증서를 획득해야 합니다. 이러한 웹 사이트는 신뢰를 구축하기 위해 데이터를 교환하기 전에 브라우저와 인증서를 공유합니다. SSL 인증서는 암호화 정보도 포함하므로 서버와 웹 브라우저는 암호화된 데이터나 스크램블된 데이터를 교환할 수 있습니다. 프로세스는 다음과 같이 작동합니다.
    
    1. 사용자 브라우저의 주소 표시줄에 *https://* URL 형식을 입력하여 HTTPS 웹 사이트를 방문합니다.
    2. 브라우저는 서버의 SSL 인증서를 요청하여 사이트의 신뢰성을 검증하려고 시도합니다.
    3. 서버는 퍼블릭 키가 포함된 SSL 인증서를 회신으로 전송합니다.
    4. 웹 사이트의 SSL 인증서는 서버 아이덴티티를 증명합니다. 브라우저에서 인증되면, 브라우저가 퍼블릭 키를 사용하여 비밀 세션 키가 포함된 메시지를 암호화하고 전송합니다.
    5. 웹 서버는 개인 키를 사용하여 메시지를 해독하고 세션 키를 검색합니다. 그런 다음, 세션 키를 암호화하고 브라우저에 승인 메시지를 전송합니다.
    6. 이제 브라우저와 웹 서버 모두 동일한 세션 키를 사용하여 메시지를 안전하게 교환하도록 전환합니다.
</div>
</details>

</br></br>
# 참고자료

[HTTP와 HTTPS 비교 - 전송 프로토콜 간의 차이점 - AWS](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)
