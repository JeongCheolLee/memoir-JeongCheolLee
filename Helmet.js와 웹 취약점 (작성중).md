

## 1. 헬멧이 뭘까?
 ![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/fa8b21dg38aj6.jpeg)
이 글은 당연히 머리를 보호하고자 사용하는 헬멧에 대한 글은 아니다. 사실 맞다고도 볼 수 있다. 
Helmet은 protect, http header와 관련이 있기 때문이다. 
Nest에선 헬멧을 아래와 같이 설명하고 있다.

> Helmet은 HTTP 헤더를 설정하여 잘 알려진 웹 취약점으로부터 앱을 보호합니다. 일반적으로 Helmet은 보안 관련 HTTP 헤더를 설정하는 작은 미들웨어 기능 모음입니다.

## 2. 헬멧은 어떤 '잘 알려진 웹 취약점'으로 부터 보호할까? 
Helmet의 공식 문서에 따르면, app.use(helmet()); 만 추가해도 아래와 같은 헤더를 세팅한다고 한다.

#### 1. Content-Security-Policy
```
웹 사이트가 실행되는 동안 허용된 리소스의 종류 및 출처를 명시적으로 정의한다.
이를 통해 XSS같은 다양한 웹 공격을 방지할 수 있다.
```
 
#### 2. Cross-Origin-Opener-Policy
```

```


3. Cross-Origin-Resource-Policy: Blocks others from loading your resources cross-origin
4. Origin-Agent-Cluster: Changes process isolation to be origin-based
5. Referrer-Policy: Controls the Referer header
6. Strict-Transport-Security: Tells browsers to prefer HTTPS
7. X-Content-Type-Options: Avoids MIME sniffing
8. X-DNS-Prefetch-Control: Controls DNS prefetching
9. X-Download-Options: Forces downloads to be saved (Internet Explorer only)
10. X-Frame-Options: Legacy header that mitigates clickjacking attacks
11. X-Permitted-Cross-Domain-Policies: Controls cross-domain behavior for Adobe products, like Acrobat
12. X-Powered-By: Info about the web server. Removed because it could be used in simple attacks
13. X-XSS-Protection: Legacy header that tries to mitigate XSS attacks, but makes things worse, so Helmet disables it
