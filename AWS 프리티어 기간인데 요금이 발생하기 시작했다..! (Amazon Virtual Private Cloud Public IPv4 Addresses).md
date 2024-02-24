
## 무슨일?
평화로운 어느 날.. 우연히 들어간 AWS 콘솔에서 비용이 발생하고 있는 걸 확인했다. 
메모아는 8월에 오픈 이후 비용이 발생한 적이 없었는데??
비용 분석 탭을 보니 Amazon Virtual Private Cloud (VPC) 관련 비용이 발생하고 있는 것 같았다.
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/bihie35d22ecf.png)


## 자세히 알아보자..
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/8d3e369b3505h.png)

$0.005 per In-use public IPv4 address per hour라는 항목으로 $5.45가 청구된 것을 확인할 수 있다. 

엇.. ip 주소도 돈을 냈었나..? 왜..?

원인 파악을 위해 검색 중 아래의 글을 발견했다. 
[공지 – AWS Public IPv4 주소 요금 변경 및 Public IP Insights 기능 출시](https://aws.amazon.com/ko/blogs/korea/new-aws-public-ipv4-address-charge-public-ip-insights/)
내용을 요약하자면, IPv4 주소는 고갈되어 가고 취득하는데도 돈이 드니 24년 2월 1일부터 과금을 하겠다!라는 내용이다.

IPv4가 유료 서비스로 전환되며 요금이 발생했구나! 
그런데, 내 기억에는 EC2와 연결시킬 탄력적 ip 외에는 따로 ip를 발급받은 기억이 없는데 무려 544 + 1089 = 1633h의 사용량이 발생했다. 이는 약 68일에 해당하는 시간이니.. 3개의 ip에서 비용이 발생하고 있다는 것을 알 수 있다.


## IPv4가 유료 인건 알겠는데.. 내가 3개나 발급 받았다는 거야?
다행이도 AWS에서 내가 발급받은 퍼블릭 ip에 대한 정보를 알려주는 기능이 출시된 것 같다.
Amazon VPC IP Address Manager라는 서비스인데, 콘솔에서 address 검색하면 빠르게 접근할 수 있다.

내 계정에 할당된 IPv4를 보니..
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/3h1c0b4abh8c4.png)

2개의 서비스 관리형 + 1개의 Amazon 소유 EIP! 진짜 3개다.
찾아보니 EIP는 직전에 언급한 탄력적 IP에 해당한다고 한다. 서비스 관리형 IP는 뭘까?

서비스 탭을 보니 ALB다. ALB는 최소 2개의 AZ(Availability Zone)에 걸쳐 만들어져야 하기에 2개의 퍼블릭 Ip가 생성된 것이다. (혹여나 공짜라서 4개의 AZ에 걸쳐 만들었다면, 2개로 줄여 비용 절감을 할 수 있다.)

## 결론
ip 주소 하나당 1달 기준 $3.75, 약 5000원의 비용이 청구된다. 
나는 3개를 사용하고 있기에 매달 15000원이 추가로 나가게 되는데, 큰돈은 아니지만 그동안 무료였기에 쿨하게 내고 싶지도 않다.

그런데 뭐 어쩌겠어..? 광고도 달았으니 열심히 글을 써서 15000원을 벌어보자!





