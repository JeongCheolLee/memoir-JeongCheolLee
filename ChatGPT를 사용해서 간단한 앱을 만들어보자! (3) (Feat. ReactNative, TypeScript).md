![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/c104a2bh5ab7.png)
이전 이야기 : https://www.mem0ir.com/JeongCheolLee/52

# 요약 
1. 받아쓰기 게임 개발 완료
2. 1차 앱 개발 완료

# 진행
만들려던 게임은 아래와 같다.
'주어진 문자열을 입력하는 시간을 기준으로 취기를 판단한다'

그래서 GPT에게 질문했다
``` 
화면에 1차 시기부터 4차 시기까지의 텍스트 입력 시간을 기록하는 부분이 있고,  
'아래 문장을 입력해 주세요!' 라는 텍스트 아래에는 '랜덤문자열'이 나오고,
그 아래에는 텍스트 입력하는 공간이 있어야 해.
그리고  그 아래는 입력이 시작된 순간부터 경과된 시간이 나오고, 텍스트를 입력하면 시간이 멈추고 맨 위 시간에 기록되는 게임 
```

답변은 이렇게 왔다.

```
import React, { useState, useEffect } from 'react';
import { View, Text, TextInput, Button, StyleSheet } from 'react-native';

const GameScreen: React.FC = () => {
  const randomString = '랜덤문자열'; // 랜덤 문자열
  const [startTime, setStartTime] = useState<number | null>(null);
  const [elapsedTime, setElapsedTime] = useState<number>(0);
  const [inputText, setInputText] = useState('');
  const [gameStarted, setGameStarted] = useState(false);
  const [gameTimes, setGameTimes] = useState<number[]>(Array(4).fill(0));

  useEffect(() => {
    let interval: NodeJS.Timeout;

    if (gameStarted) {
      if (startTime === null) {
        setStartTime(Date.now());
        interval = setInterval(() => {
          setElapsedTime(Date.now() - (startTime || 0));
        }, 100);
      }
    } else {
      clearInterval(interval);
    }

    return () => clearInterval(interval);
  }, [gameStarted, startTime]);

  const startGame = () => {
    setGameStarted(true);
    setStartTime(Date.now());
    setElapsedTime(0);
    setInputText('');
  };

  const stopGame = () => {
    if (gameStarted) {
      setGameStarted(false);
      const elapsedTimeInSec = Math.floor(elapsedTime / 1000);
      if (elapsedTimeInSec < gameTimes.length) {
        gameTimes[elapsedTimeInSec] = elapsedTimeInSec;
        setGameTimes([...gameTimes]);
      }
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.gameText}>1차 시기</Text>
      <Text>{`아래 문장을 입력해주세요: ${randomString}`}</Text>
      <TextInput
        style={styles.input}
        value={inputText}
        onChangeText={(text) => setInputText(text)}
        placeholder="텍스트 입력"
        editable={gameStarted}
      />
      <Text>{`경과 시간: ${elapsedTime / 1000} 초`}</Text>
      <Button title={gameStarted ? '멈추기' : '시작하기'} onPress={gameStarted ? stopGame : startGame} />
      <Text>1차: {gameTimes[0]} 초, 2차: {gameTimes[1]} 초, 3차: {gameTimes[2]} 초, 4차: {gameTimes[3]} 초</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  gameText: {
    fontSize: 24,
    fontWeight: 'bold',
  },
  input: {
    width: 200,
    borderWidth: 1,
    marginTop: 10,
  },
});

export default GameScreen;

```

화면이 오류 없이 렌더 되긴 했으나, 원하는 기능의 구현은 안되었다.  그래서 그동안 개발하며 감을 나름 잡았다고 생각해 직접 구현했다.

추가적으로, 기록이나 측정 시간은 asyncStorage에 저장시켜 두고, 초기화 버튼을 누르면 clear 하도록 했다.

이왕이면 앱스토어에 배포까지 해보려고 했으나, 멤버십 가입이 유료이기에 내 기기에만 설치했다. 나름 디자인과 (색 입히기)를 끝낸 모습은 아래와 같다.
소스코드 : https://github.com/JeongCheolLee/AmIDrunk

# 결과

![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/gj1i0ibb4gbe.gif)


# 마치며..
Q. GPT는 얼마나 도움이 되었나요?


-  GPT의 대답으로만 만든 결과물만 보자면, 엄청나게 만족스럽진 않았다. (내가 프롬프트를 상세하게 작성하지 않아서 일 수도 있겠지만) 
- 다만 앱 개발 입문에는 큰 도움이 된 것 같다. 레퍼런스가 없을 땐 어떻게, 어디서 시작해야 할지 잘 모르기 마련이다. GPT는 완벽하진 않아도 완성된 코드를 응답하기 때문에 일단 코드를 받아보고, 모르는 것을 하나씩 찾아보면서 입맛에 맞게 고치는 식으로 진행하기엔 충분한 도움이 됐다.

Q. 느낀 점?
-  React Native는 아직 0.72버전이다. 정식 버전이 아니라는 의미인데, hot reload가 간혹 안 먹는다던가, 빌드가 안돼서 xcode를 다시 켜기만 했는데 빌드가 된다던가 하는 사소한 버그들이 있었다.
- 그래도 앞으로 아이디어가 떠오른다면 바로바로 만들어 볼 수 있을 것 같다.
- 내게는 새로운 기술이기도 했고, 정말 오랜만에 클라이언트 개발이기도 해서인지 재밌었다. 이번 개발처럼 맛만 보고 끝나는 게 아니라, 좋은 아이디어가 떠오른다면 백엔드까지 있는 프로젝트를 해볼 예정이다.