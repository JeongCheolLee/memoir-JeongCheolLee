새로 시작한 사이드프로젝트는 java / spring boot / jpa를 사용해보기로 했다.
사실상 처음하는 언어와 프레임워크라 모든것이 새롭다. 

GPT의 훈수에 따라 프로젝트 초기 세팅을 마치고 마찬가지로 GPT가 전해준 User entity를 작성 하자마자 아래와 같은 에러를 마주했다.

"package javax.persistence does not exist" 
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/b2e3261bie48f.png)

구글링의 결과는 아래와 같다.
[고마워요 구글!](https://mkyong.com/spring-boot/spring-boot-package-javax-persistence-does-not-exist/)

요약하자면 스프링 부트 3 버전 이후로는 JAVA EE의 관리 주체가 oracle에서 eclipse foundation으로 넘어가면서 프로젝트의 이름아 Jakarta EE로 변했다는것.

이에 맞게 javax를 jakarta로 변경해주면 된다.

![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/48bbi490ede26.png)



cf) 자세한 이야기는 아래 GPT를 참조
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/2f0695dcjhci7.png)

