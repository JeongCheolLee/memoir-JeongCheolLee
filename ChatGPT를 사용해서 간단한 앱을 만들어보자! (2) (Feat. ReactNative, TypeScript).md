![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/99fdgbj78h5j.png)

이전 이야기 : https://www.mem0ir.com/JeongCheolLee/50

# 요약
1. 게임 선택 화면 개발
2. navigation 기능 개발



# 진행

개발환경은 이미 프로젝트 시작 전에 세팅이 완료되어 있었다.
그래서 GPT에게 했던 첫 질문은 아래와 같다.


```
Q. 맨 위 영역은 심플한 맥주 같은 주류 이미지에 '지금 술을 마시고 계신가요? 아래 테스트를 통해 취하셨는지 확인해 보세요'라는 문구를 띄우고 

그 아래는 게임 선택 버튼이 있었으면 좋겠어 어떻게 해야해?
```
그에 대한 GPT의 대답은..

```
import React from 'react';
import { View, Text, Image, TouchableOpacity, StyleSheet } from 'react-native';

const HomeScreen = ({ navigation }) => {
  return (
    <View style={styles.container}>
      <Image
        source={require('./path-to-your-image.jpg')}
        style={styles.image}
      />
      <Text style={styles.text}>
        지금 술을 마시고 계신가요? 아래 테스트를 통해 취하셨는지 확인해보세요.
      </Text>
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate('Game1')}
      >
        <Text style={styles.buttonText}>게임 1</Text>
      </TouchableOpacity>
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate('Game2')}
      >
        <Text style={styles.buttonText}>게임 2</Text>
      </TouchableOpacity>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  image: {
    width: 200,
    height: 200,
    resizeMode: 'contain',
  },
  text: {
    fontSize: 18,
    textAlign: 'center',
    marginTop: 20,
  },
  button: {
    backgroundColor: 'blue', // 버튼 배경색
    padding: 10,
    borderRadius: 5,
    marginTop: 20,
  },
  buttonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default HomeScreen;

```

이후에 HomeScreen을 렌더 해보니..
![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/13059c7c2hb6g7.png)

나쁘진 않은데,, 어딘가 구림은 감출 수가 없다. 예쁘게 만드는 게 중요한 건 아니니 일단 기능 구현부터 진행해 보자

이제 버튼을 눌렀을 때 페이지 이동을 해야 하기에 질문을 남겼다.
```
Q. 네비게이션 설정은 어떻게 해?

A. 
react-navigation을 사용하여 내비게이션을 설정하려면 다음 단계를 따르세요. 이 예에서는 스택 내비게이터(Stack Navigator)를 사용하여 화면 간 이동을 구현합니다.

react-navigation 설치:
yarn을 사용하여 react-navigation 및 필요한 패키지를 설치합니다.


yarn add @react-navigation/native @react-navigation/stack
내비게이션 컨테이너 설정:
앱의 최상위 컴포넌트에 내비게이션 컨테이너를 설정합니다. 주로 App.js 또는 index.js 파일에서 이 작업을 수행합니다.

import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import HomeScreen from './screens/HomeScreen'; // 홈 화면 컴포넌트 임포트

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        {/* 다른 화면들을 여기에 추가 */}
      </Stack.Navigator>
    </NavigationContainer>
  );
}
화면 컴포넌트 구성:
각 화면을 컴포넌트로 구성합니다. 위의 예제에서는 HomeScreen을 만들어 홈 화면으로 사용했습니다.


import React from 'react';
import { View, Text, TouchableOpacity } from 'react-native';

const HomeScreen = ({ navigation }) => {
  return (
    <View>
      <Text>이곳에 홈 화면 컨텐츠를 추가하세요</Text>
      <TouchableOpacity onPress={() => navigation.navigate('Game1')}>
        <Text>게임 1로 이동</Text>
      </TouchableOpacity>
      <TouchableOpacity onPress={() => navigation.navigate('Game2')}>
        <Text>게임 2로 이동</Text>
      </TouchableOpacity>
    </View>
  );
};

export default HomeScreen;
다른 화면 추가:
다른 화면을 생성하고 내비게이션 스택에 추가하세요. Stack.Screen 컴포넌트를 사용하여 다른 화면을 정의하고, 해당 화면 컴포넌트를 component prop에 전달합니다. 이 예에서는 'Game1' 및 'Game2' 화면을 추가한 것처럼 추가 화면을 정의합니다.


<Stack.Screen name="Game1" component={Game1Screen} />
<Stack.Screen name="Game2" component={Game2Screen} />
화면 간 이동:
각 버튼 또는 사용자 이벤트에 대한 핸들러 함수 내에서 navigation.navigate 함수를 사용하여 화면 간 이동을 구현할 수 있습니다. 위의 HomeScreen.js 예제에서는 게임 선택 버튼을 누를 때 'Game1' 또는 'Game2' 화면으로 이동하는 방법을 보여줍니다.

이렇게하면 React Native 앱에서 내비게이션을 설정할 수 있습니다. 필요한 경우 다른 화면을 추가하고 내비게이션을 조정하여 전체 앱을 완성하세요.
```

여기서 문제가 좀 있었다.
설치하라고 했던 디펜던시를 모두 설치했는데,, react-native-gesture-handler, react-native-screens 등등의 디펜던시를 추가로 설치해 주어야 했다.
Game 화면을 아직 만들지 않아서 HomeScreen으로 이동하도록 수정해 테스트했는데, 잘 되는 것 같다.


# 정리
아직은 수월하다.. 시키는 대로만 해서는 안되지만, 금방 해결할 수 있는 문제들이었다.