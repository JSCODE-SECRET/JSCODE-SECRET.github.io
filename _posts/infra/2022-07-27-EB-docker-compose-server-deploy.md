---
layout: post
title: 'EB(Elastic Beanstalk), docker-compose로 서버 배포하기 (CI/CD 포함 X)'
subtitle: 'EB(Elastic Beanstalk), docker-compose로 서버 배포하기 (CI/CD 포함 X)'
categories: Infra
comments: false
---

### 1. EB 애플리케이션 만들기

![Untitled 0](https://user-images.githubusercontent.com/41244373/181147097-93f3d8b7-8b70-402a-90a9-8e29fcd0fbd4.png)
![Untitled 1](https://user-images.githubusercontent.com/41244373/181147103-a44d42cd-e9b9-4e43-8e51-a39dbf3d982e.png)

### 2. EB 애플리케이션의 환경 생성

![Untitled 2](https://user-images.githubusercontent.com/41244373/181147280-e0bf3a3c-40ea-4e0c-b128-650acbd7ae7a.png)
![Untitled 3](https://user-images.githubusercontent.com/41244373/181147289-5069d55a-3e2a-474f-935a-ad2c79ed13a4.png)

### 3. ECR에 접근할 수 있는 IAM 정책 만들기

![Untitled 4](https://user-images.githubusercontent.com/41244373/181147318-75fc26ad-4330-43df-867e-729e6eb26be6.png)
![Untitled 5](https://user-images.githubusercontent.com/41244373/181147328-036e782e-7d34-444e-8a57-5193f2f2c806.png)
![Untitled 6](https://user-images.githubusercontent.com/41244373/181147332-d5965c83-fde5-4bdc-a2e9-6c434e03b574.png)
![Untitled 7](https://user-images.githubusercontent.com/41244373/181147336-999250d6-6cf1-450b-8612-c51eb4615f75.png)
![Untitled 8](https://user-images.githubusercontent.com/41244373/181147339-743c6cf8-c311-43ad-ba50-258b678961c7.png)
![Untitled 9](https://user-images.githubusercontent.com/41244373/181147345-7ad9294b-a369-43b8-af5d-a5bdbda4f1ec.png)
![Untitled 10](https://user-images.githubusercontent.com/41244373/181147346-d7e897a9-5e1c-4eee-b507-e51f2739b9ed.png)

### 4. Elastic Beanstalk에서의 EC2가 쓸 IAM 만들기

![Untitled 11](https://user-images.githubusercontent.com/41244373/181147395-24361bc9-3997-4dbc-b2d9-bcc497ad6763.png)
![Untitled 12](https://user-images.githubusercontent.com/41244373/181147407-95364ef9-6bb1-4c60-b7c9-3ef6dd2c7261.png)
![Untitled 13](https://user-images.githubusercontent.com/41244373/181147411-6d892908-967d-4840-b082-bd22f9ec409c.png)
![Untitled 14](https://user-images.githubusercontent.com/41244373/181147414-90e18d9f-3197-40ed-8555-1f65f2ddb3d8.png)

> ✅ Default로 생성되는 aws-elasticbeanstalk-ec2-role 역할에 있던 정책들도 같이 추가해줘야, 기존에 EB의 EC2가 수행하던 ‘상태’에 관련된 로그를 수집한다던가 여러가지 기능들을 정상적으로 수행할 수 있다.

![Untitled 15](https://user-images.githubusercontent.com/41244373/181147465-a988a602-aa77-4f29-93fe-8ea20ba8c79c.png)

> ⛔ **주의)** 아래의 그림과 같이 Elastic Beanstalk를 만들면 Default로 생성되는 역할에 위에서 방금 만든 ECR_FullAccess 정책을 추가해주더라도 작동을 하지 않는다. 이유는 모르겠다. 따라서 Default로 만들어진 IAM 역할을 쓰지 말고, IAM 역할을 실제로 만들어서 사용하자. 

> **여담)** StackOverflow의 답변 중에서 아주 작게 달려있는 Comment에서 어떤 사람이 Default IAM을 사용하지 않았더니 해결되었다는 말을 보게 되어서, 덕분에 나도 해결할 수 있었다.


![Untitled 16](https://user-images.githubusercontent.com/41244373/181147476-7b8c175e-10b1-4786-8c7d-d79e1f5b2e44.png)

### 5. Elastic Beanstalk에서의 EC2가 쓸 IAM 설정하기
→ 생성된 EB 애플리케이션의 환경의 ‘구성’ 변경

![Untitled 17](https://user-images.githubusercontent.com/41244373/181147484-8833bfc1-f0e1-4c65-a465-c9f10c70f550.png)
![Untitled 18](https://user-images.githubusercontent.com/41244373/181147488-cb310f61-5c42-4472-87dc-29258e420dda.png)

### 6. EB에 docker-compose 형태로 배포하기 (+ 개발 환경, 배포 환경 분리)

> ✅ docker-compose 파일의 이름을 `docker-compose.yml`으로 쓰지 않아도, 파일 내부의 코드가 docker-compose의 형식을 따르고 있으면 EB 자체에서 `docker-compose.yml`로 인식을 한다.

docker-compose.dev.yml

```bash
version: "3.9"
services:
  server:
    container_name: onlyone-server
    image: "_______.dkr.ecr.ap-northeast-2.amazonaws.com/onlyone-dev:latest"
    ports:
      - "80:3000"
    environment:
      - NODE_ENV=development
```

docker-compose.prod.yml

```bash
version: "3.9"
services:
  server:
    container_name: onlyone-server
    image: "_______.dkr.ecr.ap-northeast-2.amazonaws.com/onlyone-dev:latest"
    ports:
      - "80:3000"
    environment:
      - NODE_ENV=prouction
```

> ⛔ AWS의 공식문서에 따르면 docker-compose에서 EB의 환경 변수를 사용하고 싶으면, docker-compose 파일 내에 `env_file: .env`의 속성을 추가해주라고 한다. 하지만 추가해서 EB에 docker-compose 파일을 넣으면, EB가 Dockerfile이라고 인식해버려서 제대로 작동하지 않는다. 이유는 잘 모르겠다. EB 내부 버그인 것으로 추정된다.

### (주의사항)

- EB에서 셋팅되는 로드밸런스의 Health Check는 `/ GET` 요청을 보냈을 때 `200`이라는 응답이 와야 정상으로 판단되게끔 셋팅되어 있다.

# References

- [Docker 구성](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/single-container-docker-configuration.html)
- [Docker 플랫폼 브랜치 사용](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/docker.html)
- [[AWS, Github Action] Elastic Beanstalk에 SpringBoot 이미지 Docker로 배포하기(2) - ECR 리포지토리 생성 및 권한 설정](https://earth-95.tistory.com/122)