Github Action은 깃허브에서 제공하는 배포 서버(github-hosted runner)를 사용하거나, 본인의 배포 서버(self-hosted runner)를 사용하는 두 가지 방법이 있다.

github-hosted runner 경우엔 IP를 특정할 수 없기에(매번 바뀜) EC2에 인바운드 정책을 설정하기가 까다로우니, 본인의 EC2를 self-hosted runner로 사용해 CI/CD를 구현해보자

가장 먼저 레포지토리 settings 에서 self-hosted runner를 등록해줘야한다.
settings -> actions -> runners 순으로 들어가 new self-hosted runner 버튼을 눌러서 생성하면 된다.

![image](https://d1ccleacxg8gcm.cloudfront.net/JeongCheolLee/images/9f03234eeg96.png)


대부분 EC2의 OS는 리눅스로 쓸 테지만, 본인이 self-hosted runner로 사용할 서버의 image, architecture를 골라준다.

이후 각각 나와있는 명령어를 ec2에 접속해서 하나씩 실행시키면 된다.
마지막에 action-runner를 실행 시킬 땐, 종료되지 않고 백그라운드에서 실행하도록 nohup 명령어와 &를 활용하자

이후엔 본인 코드에서 workflows 폴더를 추가하거나, 깃허브 콘솔을 통해 추가하면 된다.
아래는 pm2를 활용해 코드를 재시작하는 yml파일이다.

```

// /.github/workflows/prod.yml

name: Deploy to PROD EC2

on:
  push:
    branches: ["main"]

permissions:
  contents: read

jobs:
  deploy:
    name: Deploy
    runs-on: prod-self-hosted

    steps:
      - name: Git Pull
        id: gitpull
        working-directory: /home/ubuntu/memoir-server
        run: |
          git pull origin main

      - name: Install dependency
        id: install
        working-directory: /home/ubuntu/memoir-server
        run: |
          npm install

      - name: build
        id: build
        working-directory: /home/ubuntu/memoir-server
        run: |
          npm run build

      - name: PM2 reload
        id: reload
        working-directory: /home/ubuntu/memoir-server
        run: |
          npm run reload:prod


```

본인이 배포시 입력하는 스크립트에 맞게 변형해서 사용하면 된다.