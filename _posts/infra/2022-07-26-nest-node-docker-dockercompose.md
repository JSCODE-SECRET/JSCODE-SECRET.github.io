---
layout: post
title: 'Nest.js(Node.js)를 Docker, docker-compose로 실행하기'
subtitle: 'Nest.js(Node.js)를 Docker, docker-compose로 실행하기'
categories: Infra
comments: false
---

> ✅ Docker 이미지를 바탕으로 배포할 때에 필수적인 Push하고 Pull 하는 과정은 빠져있다. 단순히 Docker로 Node.js의 서버를 실행시키는 방법만이 포함되어 있다.

# 1. Dockerfile, docker-compose.yml 파일 작성

### 폴더 구성

![Nest.js(Node.js)를 Docker, docker-compose로 실행](https://user-images.githubusercontent.com/41244373/180907021-4499b122-568a-455d-99ba-0b5a3346951e.png)

> ✅ Dockerfile, docker-compose.yml은 배포 환경에서 사용되고, Dockerfile.dev, docker-compose.yml은 개발 환경에서 사용된다.

### .dockerignore

```docker
node_modules/
```

- `node_modules`는 `Dockerfile`에서 `COPY`를 할 때 복사되지 않는다. `node_modules`는 용량이 너무 크기 때문에 `COPY`를 해서 이미지로 만드는 것보다, 베이스 이미지 내에서 `$ npm install`로 모듈을 새로 받아서 이미지를 만드는 것이 훨씬 효율적이다.

### Dockerfile

```
FROM node:16-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY ./ ./

RUN npm run build

ENV NODE_ENV production

CMD ["node", "dist/main.js"]
```

- `FROM node:16-alpine`
    - 이미지를 빌드하기 위한 베이스 이미지
    - Node의 16버전을 사용하기 위해 Node의 16버전에 해당하는 Docker 이미지를 사용했다.
    - `alpine`은 이미지 중에서 가장 작은 사이즈로 정말 필요한 것들만 담겨져 있는 이미지를 의미한다. 최적화된 용량의 이미지를 활용해 불필요한 리소스의 사용을 줄이기 위해서 `alpine`의 이미지를 사용했다.
- `WORKDIR /usr/src/app`
    - Docker 이미지를 만들 때 `WORKDIR`를 명시 해주지 않으면, `COPY`라는 명령어를 통해 소스 코드 파일들을 베이스 이미지 안으로 복사할 때 기존 베이스 이미지에 있던 파일들끼리 덮어씌워지거나 엉키는 경우가 가끔 발생한다. 따라서 `/usr/src/app`(해당 주소는 바꿔도 됨)이라는 `WORKDIR`을 명시해줌으로써, 베이스 이미지 내의 `/usr/src/app`에 소스 코드 파일들이 복사된다.
    - 참고 : [https://minjoon950425.tistory.com/132](https://minjoon950425.tistory.com/132)
- `COPY package*.json ./`
    - `package.json`, `package-lock.json` 파일을 베이스 이미지 안으로 복사한다.
- `RUN npm install`
    - 베이스 이미지 안에 `package.json` 파일이 있으니, 베이스 이미지 안에서 `$ npm install`로 의존 관계에 있는 모듈을 설치할 수 있다.
- `COPY ./ ./`
    - 소스 코드 파일 전체(`.dockerignore`에 의해 제외된 파일은 포함하지 않음)를 베이스 이미지 안으로 복사한다.
- `RUN npm run build`
    - 베이스 이미지 안에서 소스 코드 파일 전체가 `COPY` 됐으므로, 베이스 이미지 안에서 `$ npm run build`를 통해 빌드를 할 수 있다.
- `ENV NODE_ENV production`
    - `NODE_ENV`의 환경변수는 배포환경이기 때문에 production으로 설정해준다.
- `CMD`
    - 베이스 이미지 내에서 실행되는 명령어가 아니라, 해당 이미지가 컨테이너화 됐을 때 최초로 실행되는 명령어를 의미한다. (RUN과 CMD가 다른 점은, 베이스 이미지 내에서 실행되느냐, 아니면 이미지가 컨테이너로 실행됐을 때 실행되느냐의 차이이다.)

### docker-compose.yml

```docker
version: "3.9"
services:
  server:
    container_name: onlyone-prod-server
    image:
      "jaeseongdev/onlyone-prod-servrer"
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
```

- `container_name` : 이미지를 컨테이너화 시킬 때 붙여줄 컨테이너 이름
- `image` : pull해서 사용할 이미지 이름 (먼저 local에 있는 지 체크하고, local에 없다면 dockerhub에 있는 지 체크한다.)

### Dockerfile.dev

```docker
FROM node:16-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY ./ ./

RUN npm run build

ENV NODE_ENV development

CMD ["node", "dist/main.js"]
```

### docker-compose.dev.yml

```docker
version: "3.9"
services:
  server:
    container_name: onlyone-dev-server
    image:
      "jaeseongdev/onlyone-dev-servrer"
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
```

# 2. Dockerfile을 바탕으로 이미지 빌드하기

### Dockerfile을 바탕으로 이미지 빌드하기

```docker
$ docker build -t jaeseongdev/onlyone-prod-servrer ./
```

### Dockerfile.dev를 바탕으로 이미지 빌드하기
(Dockerfile.dev가 있는 디렉토리에서 아래의 명령어를 실행해야 함.)

```docker
$ docker build -f Dockerfile.dev -t jaeseongdev/onlyone-dev-servrer ./
```

# 3. docker-compose를 활용해 빌드된 이미지로 컨테이너 실행시키기

### 배포 환경인 docker-compose.yml로 컨테이너 실행시키기

```docker
$ docker-compose -f docker-compose.yml up -d
```

### 개발 환경인 docker-compose.dev.yml로 컨테이너 실행시키기

```docker
$ docker-compose -f docker-compose.dev.yml up -d
```

> 위 명령어를 실행시키면, `$ docker ps`를 통해서 제대로 서버가 실행되고 있는 것을 확인할 수 있다. 아니면 localhost:3000으로 해당 서버가 잘 응답하는 지 테스트해봐도 좋다.
