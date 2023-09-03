![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/85b5hbeig46j6.png)

서드파티 API를 연동하거나, 속성이 많은 클래스를 다룰 때 "필요한 속성만 필터링할 순 없나..?"라는 생각이 들었다.

간단히 예를 들면 아래와 같은 상황이다.

![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/edaf307c9b237.png)

위 이미지처럼 이름과 나이가 정의된 UserDto가 내가 필요한 상황이고, 서드파티 API의 리스폰스가 아래와 같다고 가정하자

```
    const user = {
      name: "손흥민",
      age: 30,
      team: "Spurs",
      nationality: "Korea",
      goals: 27,
      passes: 3424,
    };

```


나는 name, age만 필요한데..라고 한다면 간단히 아래와 같이 할 수 있을 것이다.

```

    const temp = new UserDto();

    temp.age = user.age;
    temp.name = user.name;

    console.log(temp); // UserDto { age: 30, name: '손흥민' }
```

원하는 결과는 얻었지만, 속성이 많아질수록 매우 번거로워진다.

그럴 때 사용하는 것이 class-transformer의 plainToInstance 메소드와 excludeExtraneousValues 옵션이다.

사용법은 아래와 같다.

1. DTO에서 필요한 속성에 @Exclude() 데코레이터를 달아준다.
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/0hj6e9g7f6a0e.png)


2. transform이 필요한 곳에서 아래와 같이 사용한다.

```
    const transformedData = plainToInstance(UserDto, user, {
      excludeExtraneousValues: true,
    });

    console.log(transformedData) // UserDto { name: '손흥민', age: 30 }

```





