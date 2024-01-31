## <div align="center">CORS</div>
<div align="center">마지막으로 공부한 날짜 : 2022.12.14</div>
</br></br>

# CORS (**Cross-Origin Resource Sharing**)

- 서로 다른 도메인간에 자원을 공유하는 것을 의미하며 기본적으로 차단되어있음

```markdown
🚨 Access to fetch at ‘https://api.lubycon.com/me’ from origin ‘http://localhost:3000’ has been blocked by CORS policy: No ‘Access-Control-Allow-Origin’ header is present on the requested resource. If an opaque response serves your needs, set the request’s mode to ‘no-cors’ to fetch the resource with CORS disabled.
```

</br>

## URL

```markdown
scheme://host:port/path?query
```
![URL 구조](https://github.com/fsm12/Ready-For-Tech-Interview/assets/74345771/cd8f08e1-bf96-405e-8ce6-5e44e6bf407c)

|종류|Protocol|Host|Port|Path|Query|Fragment|
|---|---|---|---|---|-----|---|
| 설명 | 통신 방법에 대한 규칙 | 리소스가 있는 시스템의 이름; 하위→최상위 도메인 순으로 작성 | 연결할 포트 번호 (일반적으로 옵션) | 파일 또는 리소스의 정확한 위치를 지정 | 검색의 특정 매개 변수 | HTML 요소에 대한 'id' 또는 'name' 속성과 같은 페이지 내의 특정 위치 |
| 형식 | {PROTOCOL}:// | {HOST} | :{PORT} | /{PATH} | ?{QUERY_STRING} | #{FRAGMENT} |

- Origin(==Protocol + Host + Port) 이 같다면 CORS 에러는 발생하지 않음
- 출처확인방법

    ```markdown
    console.log(location.origin);   # https://github.com
    ```
    
</br>

## SOP (Same Origin Policy)

- 같은 출처여야 함 → 다른 출처의 리소스를 사용하는 것을 제한하는 보안 방식
- 인증된 사용자의 토큰을 탈취하더라도 지금 접속해 있는 사이트에서 출처가 다른 사이트로 어떤 특정 정보를 가져오는 요청을 보내고 그에 대한 응답이 돌아왔을 때, 브라우저는 이 응답이 출처가 다른 외부에서 날라 온 데이터라고 판별하고 **클라이언트는 응답을 받아 볼 수 없게됨**
- 하지만 외부 사이트로의 요청이 많이 필요 없었던 옛날과 달리, 이제는 내가 접속하고 있는 사이트에서 다른 사이트들과 많은 상호작용이 필요해졌기 때문에 이를 해결할 방법이 필요해졌고 이러한 상황을 해결하기 위해 나타난 것이 **CORS**
    
</br>

## CORS (**Cross-Origin Resource Sharing**)

- [ MDN 정의 ] 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제입니다. 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때 교차 출처 HTTP 요청을 실행합니다
- 즉, **CORS는 HTTP 통신을 요청할 때 HEADER에 CORS와 관련된 정보를 추가적으로 입력하여 다른 출처의 자원에 접근할 수 있도록 하는 것**

</br></br>

# 📃 CORS 접근제어 시나리오
## Simple Request
- 바로 본 요청을 보냄
- 클라이언트에서 서버에 `Origin` 값을 보냈을 때 서버는 이에 대한 응답으로 `Access-Control-Allow-Origin` 값을 보내주고 브라우저가 `CORS` 정책 위반여부를 검사
- 단순 요청은 예비 요청이 필요없는 말 그대로 단순 요청이나, 단순 요청이 일어나는데에는 아래와 같은 조건이 존재하며 **모두** 충족되어야 단순 요청이 가능해짐
    1. 요청의 Method 가 `GET`, `POST` , `HEAD` 중에 하나여야 한다.
    2. 수동으로 설정할 수 있는 헤더는 오직 [Fetch 명세에서 “CORS-safelisted request-header”로 정의한 헤더](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)뿐
        - `Accept` , `Accept-Language` , `Content-Language` , `Content-Type`
    3. `Content-Type`을 사용하는 경우에는 아래의 값 들만 허용
        - `application/x-www-form-urlencoded` , `multipart/form-data` , `text/plain`
    
    ⇒ 위 조건을 모두 다 충족 시킨다는 것은 쉽지 않기 때문에 사실상 **단순 요청**이 가능한 상황은 많지 않음
    
![image](https://github.com/fsm12/Ready-For-Tech-Interview/assets/74345771/e62a4fe6-e0d0-4743-bce0-fd8204ef0da0)

자격 증명이 있는 리소스를 요청했을 때, `Access-Control-Allow-Credentials: true` 라는 헤더가 리소스가 포함된 응답에 존재하지 않으면, 브라우저에서는 그 응답을 무시하고 웹 콘텐츠로 응답을 반환 X

</br></br>

## Preflight Request
- 유효하지 않은 요청이라면 굉장히 불필요한 리소스 낭비, 서버에도 부하가 큼
- 현재 보내는 요청이 유효한지를 확인하기 위해 `OPTIONS` 메서드로 예비 요청을 보내는 것
- 클라이언트가 서버에 요청을 시작할 때마다 브라우저는 요청에 CORS 프리플라이트가 필요한지 여부를 확인함

![image](https://github.com/fsm12/Ready-For-Tech-Interview/assets/74345771/6c65c9ff-c22b-49b8-b578-e403e78b8e5d)

```markdown
### 요청
Origin: 출처 (현재 Request를 보내는 사이트)
Access-Control-Request-Method : 본 요청에서 보내질 HTTP METHOD
Access-Control-Request-Headers : 본 요청에서 보내질 HTTP의 HEADERS

### 응답
Access-Contorl-Allow-Origin : 서버가 허락한 출처들 (Protocol + Host + Port)
Access-Control-Allow-Methods : 서버에서 허락한 HTTP Method
Access-Control-Allow-Headers : 서버에서 허용되는 HEADERS
Access-Control-Max-Age : preflight 요청에 대한 응답의 수명 (초 단위)

- 브라우저는 preflight 요청을 캐싱하여 해당 시간 동안은 다시 preflight 요청을 보내지 않고 바로 본 요청을 보내는 것이 가능해짐
```


</br>

## Credentialed Request
- 인증 관련 헤더를 포함할 때 사용하는 요청     ex) 쿠키, Jwt 토큰
- CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법
- 기본적으로 출처가 다른 경우에는 쿠키나 헤더에 Authorization 항목이 있는 요청을 보낼 수 없음
- 가능하도록 하려면 프론트와 서버 양측 모두 CORS를 설정해야 함

```markdown
### client
credentials : include  

### server
Access-Control-Allow-Credentials : true
Access-Control-Allow-Origin : * (X)
```



</br></br>

# 관련 질문

<details>
<summary>CORS는 무엇인가요?</summary>
<div markdown="1">
    
    브라우저는 전통적으로 SOP(Same Origin Policy, 동일 출처 정책)를 가지고 있음
    
    따라서, 내가 지금 접속해 있는 사이트에서 출처가 다른 사이트로 어떤 특정 정보를 가져오는 요청을 보내고 그에 대한 응답이 돌아왔을 때, 브라우저는 이 응답이 출처가 다른 외부에서 날라 온 데이터라고 판별하고 클라이언트는 응답을 받아 볼 수 없게됨
    
    이러한 상황을 해결하기 위해 나타난 것이 **CORS**
    
    HTTP 헤더를 사용해서 특정 출처에서 실행중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 방식

</div>
</details>

<details>
<summary>Preflight 요청은 왜 필요한가요?</summary>
<div markdown="1">
    
    CORS spec이 생기기 이전에 만들어진 서버들은 브라우저의 SOP(same origin policy) request만 가능하다는 가정하에 만들어짐
    다른 출처로부터의 요청이 CORS로 인해서 가능해졌는데, 전 서버들은 다른 출처에서 접근할 수 있는 보안 메커니즘이 없다 보니 보안적으로 문제가 생길 수 있어 이런 서버들을 보호하기 위해 CORS에 preflight request를 포함한 것
    Preflight request로 서버가 CORS를 인식하고 핸들 할 수 있는지 먼저 확인을 함으로써 CORS를 인식하지 못하는 서버들을 보호할 수 있는 메커니즘

</div>
</details>

<details>
<summary>Access-Control-Max-Age은 무슨역할을 하나요?</summary>
<div markdown="1">
    
    preflight 요청에 대한 응답의 수명 (초 단위)
    
    브라우저는 preflight 요청을 캐싱하여 해당 시간 동안은 다시 preflight 요청을 보내지 않고 바로 본 요청을 보내는 것이 가능해짐

</div>
</details>

<details>
<summary>CORS 에러 해결 방법은 뭔가요?</summary>
<div markdown="1">
    
    1. **프론트 프록시 서버 설정**
    
    웹 브라우저가 서버에 요청을 보낼 때, HTTP 헤더에 `origin`이라는 필드를 추가하여 자신의 출처를 알림
    
    2. **직접 헤더에 설정**
    
    서버에서 응답 헤더에 특정 헤더를 포함하는 방식으로 해결할 수 있습니다.
    
    Access-Control-Allow-Origin: 특정 브라우저가 리소스에 접근이 가능하도록 허용합니다.
    
    Access-Control-Allow-Method: 특정 HTTP Method만 리소스에 접근이 가능하도록 허용합니다.
    
    Access-Control-Expose-Headers: 헤더에 접근할 수 있도록 허용합니다.
    
    credentials: 쿠키 등의 인증 정보를 전달할 수 있습니다.
    
    3. **백엔드에서 설정**
    
    서버가 응답을 보낼 때, HTTP 헤더에 `Access-Control-Allow-Origin`이라는 필드를 추가하여 허용된 출처를 알림
    ******
  
</div>
</details>


</br></br>

# 참고자료

[CORS란 무엇입니까? - 교차 오리진 리소스 공유 설명 - AWS](https://aws.amazon.com/ko/what-is/cross-origin-resource-sharing/)



</br></br>
