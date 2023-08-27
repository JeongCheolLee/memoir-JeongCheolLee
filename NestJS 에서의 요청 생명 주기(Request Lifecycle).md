### 공식 문서에 따르면 요청 수신부터 응답까지 일반적으로 아래와 같은 순서로 진행된다고 한다.

1. Incoming request
2. Middleware
3. Guard
4. Interceptors (pre-controller)
5. Pipes
6. Controller (method handler)
7. Service (if exists)
8. Interceptors (post-request)
9. Exception filters
10. Server response


-----------
### Q. 그럼 각각 무슨일을 하는걸까?


#### - Middleware

- 코드를 실행시킴
- 요청 및 응답 object 변경 가능
- 요청 - 응답 주기 종료
- 다음 미들웨어 호출 (현재 미들웨어가 요청-응답 주기를 종료하는 기능을 포함하지 않은다면, next()를 호출해야한다. 호출하지 않으면 요청이 보류됨.

요청 처리의 맨 앞단에 위치하기 때문에 대체로 요청에 대한 로그를 남기거나, 토큰을 decode 하기 전 request object에 토큰 정보를 넣어두기도 한다.

#### - Guard

미들웨어 다음으로 실행되는데, 이름에 맞게 어플리케이션을 보호한다.
주로 요청이 해당 기능을 사용할 수 있는 권한이 있는지 체크한다.

그냥 미들웨어에서 하면 되는 거 아닌가?라는 생각이 들 수 있는데, 미들웨어는 이후 어떤 컨트롤러에 의해 요청이 처리되는지 알 수 없지만, 가드는 ExecutionContext 인스턴스에 접근 가능하기 때문에 이후 어떤 요청이 처리되는지 알 수 있기 때문에 가드가 필요하다.

예컨대, "로그인하지 않은 유저는 게시글을 등록할 수 없음" 을 구현해야 한다면 게시글 업로드 컨트롤러에 유저의 세션을 체크하는 가드를 배치해서 해당 요청이 로그인을 한 유저로부터 온 요청인지 확인할 수 있다.


#### - Interceptors (pre-controller)
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/gd00f1553648.png)
출처 : https://docs.nestjs.com/interceptors

이미지에서 알 수 있듯이, Route Handler 전/후에 위치하기 때문에 요청 응답에 대한 로그, 혹은 컨벤션에 맞춰 응답 transform 등의 역할을 담당하기에 적합학다.

#### - Pipes
파이프는 아래와 같은 역할을 한다.

1. transformation: input 데이터를 원하는 형태의 데이터로 변환한다. ex) query string으로 들어온 문자열 숫자를 넘버 형태로 변환.
2. validation: 입력된 인자가 원하는 형태인지 validation 한다.


#### - Controller
어떤 path의 요청을 받아들일지 정의한다. 컨트롤러에 정의되지 않은 요청은 받지 않는다.

#### - Service
비즈니스 로직을 수행한다. 


#### - Exception filters
요청을 처리중 에러가 발생한다면 NEST에 내장된 ExceptionFilter 에서 이를 처리해 response하게 된다. 일반적으로 에러 로그를 DB에 쌓거나 response schema에 맞게 커스텀 해서 사용한다.

