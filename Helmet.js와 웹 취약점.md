

## 1. 헬멧이 뭘까?
 ![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/fa8b21dg38aj6.jpeg)
이 글은 당연히 머리를 보호하고자 사용하는 헬멧에 대한 글은 아니다. 사실 맞다고도 볼 수 있다. 
Helmet은 protect, http header와 관련이 있기 때문이다. 
Nest에선 헬멧을 아래와 같이 설명하고 있다.

> Helmet은 HTTP 헤더를 설정하여 잘 알려진 웹 취약점으로부터 앱을 보호합니다. 일반적으로 Helmet은 보안 관련 HTTP 헤더를 설정하는 작은 미들웨어 기능 모음입니다.

## 2. 그럼 헬멧은 어떤 위험으로부터 보호할까? 
Helmet의 공식 문서에 따르면, app.use(helmet()); 만 추가해도 아래와 같은 헤더를 세팅한다고 한다. default 세팅의 상세에 대해선 [Helmet.js의 공식문서](https://helmetjs.github.io/)에서 확인 가능하다. 간단히 어떤 기능을 하는지에 대해 알아보자.

#### 1. Content-Security-Policy(CSP)
```
웹 사이트가 실행되는 동안 허용된 리소스의 종류 및 출처를 명시적으로 정의한다.
이를 통해 XSS같은 다양한 웹 공격을 방지할 수 있다.
```
 
#### 2. Cross-Origin-Opener-Policy(COOP)
```
이 헤더를 통해 브라우저는 다른 출처에서 열린 창이나 워커와의 상호 작용을 어떻게 처리할지를 제어할 수 있다.
즉 광고, 팝업 등 다른 도메인의 창에서 열린 페이지와의 상호 작용을 제한할 수 있다.
```
#### 3. Cross-Origin-Resource-Policy(CORP)
```
2번 COOP와 비슷한데, CORP는 리소스 로딩에 중점을 두고, 어떤 출처에서 리소스를 사용할 수 있는지 제어한다. 반면에 COOP는 창 간 상호 작용에 중점을 두고, 다른 출처의 창과의 상호 작용을 어떻게 허용할지를 제어한다. 
```
#### 4. Origin-Agent-Cluster
```
Origin-Agent-Cluster 헤더는 웹 애플리케이션이 원본을 다른 프로세스로부터 격리할 수 있도록 한다.
```
#### 5. Referrer-Policy
```
Referrer 헤더를 제어한다. 사용자가 어디에서 왔는지를 숨길 수 있다.
```
#### 6. Strict-Transport-Security
```
브라우저에게 HTTPS 통신을 사용하라고 알려준다.
```
#### 7. X-Content-Type-Options
```
MIME sniffing을 방지한다. 
```
cf) [MIME sniffing 이란?](https://velog.io/@liankim/HTTP-MIME-Type-fnpj37ed)
#### 8. X-DNS-Prefetch-Control
```
DNS 사전조회(DNS prefetching)를 어떻게 처리할지 제어하는 헤더.
DNS 사전조회는 사용자가 링크를 클릭하기 전에 해당 링크의 도메인에 대한 DNS 조회를 브라우저가 미리 수행하는 것을 의미한다.
```
#### 9. X-Download-Options
```
이 헤더는 Internet Explorer 8에만 해당된다고 한다. (몰라도 될 것 같다고 한다.)
브라우저에서 다운로드(저장)기능만 제공하도록 제어한다.
```
#### 10. X-Frame-Options
```
클릭재킹 공격을 완화하는 헤더.
CSP로 대체되었지 이전 브라우저나 CSP가 사용되지 않는 경우에 사용할 수 있다.
```
#### 11. X-Permitted-Cross-Domain-Policies: 
```
이 헤더는 일부 클라이언트(주로 Adobe 제품)에 도메인 간 콘텐츠 로드에 대한 도메인 정책을 알려주는 역할을 한다.
```
#### 12. X-Powered-By
```
웹 서버에 대한 정보를 나타내는 헤더인데, 굳이 외부에 노출될 필요 없으므로 삭제하자.
예를들어 express로 제작된 백엔드 애플리케이션이라는게 나타나게 되는데, express에 보안 위험이 발견되면 마찬가지로 내 애플리케이션이 공격 대상이 될 수 있다.
```
#### 13. X-XSS-Protection
```
XSS 공격을 막기 위해 만들어졌지만, 버그가 있는지 helmet에서 설정을 끄고, CSP를 사용하는걸 권장하는 것 같다.
```

