---
layout: post
title: 'Elastic Beanstalk가 제대로 실행되지 않아서 디버깅을 해야 할 때 / Elastic Beanstalk 디버깅 방법'
subtitle: 'Elastic Beanstalk가 제대로 실행되지 않아서 디버깅을 해야 할 때 / Elastic Beanstalk 디버깅 방법'
categories: Infra
comments: false
---

### 1. 이벤트 확인하기

![Untitled](https://user-images.githubusercontent.com/41244373/181148117-0b9622ac-483a-410e-bfbb-cf92dc9623b5.png)

### 2. 로그 확인하기

![Untitled 1](https://user-images.githubusercontent.com/41244373/181148168-faa82b64-a2ae-4c14-b8ec-e5338387ee52.png)
![Untitled 2](https://user-images.githubusercontent.com/41244373/181148188-bc649d4d-7a1a-4c87-aee3-662190602ec0.png)

### 3. EC2에 직접 들어가서 서버 실행시켜보기

![Untitled 3](https://user-images.githubusercontent.com/41244373/181148209-c1c1f8fb-10e2-42d3-9a8d-ad5a3d5b8144.png)
![Untitled 4](https://user-images.githubusercontent.com/41244373/181148212-c0b64ba4-52d8-4cfe-8967-a01725c47559.png)

```bash
$ sudo su # root 권한으로 변경하기
$ cd /var/app/current # EB를 통해 업로드한 파일이 있는 디렉토리이다.
```

![Untitled 5](https://user-images.githubusercontent.com/41244373/181148214-ebc6cc8e-baa5-4b53-989e-2e42868e28a5.png)

> 해당 위치에서 직접 명령어를 하나하나 쳐보면서 잘 작동하는 지 디버깅 해보고, 여기서 디버깅 해봄으로써 에러 코드를 바로바로 직접 확인할 수 있어서 어디에 버그가 존재하는 지 쉽게 찾을 수 있다.