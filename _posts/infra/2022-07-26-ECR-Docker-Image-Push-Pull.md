---
layout: post
title: 'ECR을 사용해서 Docker Image를 Push, Pull하는 방법'
subtitle: 'ECR을 사용해서 Docker Image를 Push, Pull하는 방법'
categories: Infra
comments: false
---

# 1. ECR을 사용할 수 있는 IAM 권한 추가하기

![image](https://user-images.githubusercontent.com/41244373/180926612-436175fe-2adf-4086-b0ba-27e83a4f3482.png)

# 2. ECR에 Push를 할 서버(로컬 서버 or EC2 인스턴스 등)에 AWS CLI 설치하기

[최신 버전의 AWS CLI 설치 또는 업데이트](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)

# 3. AWS CLI에 인증 정보 설정해주기

```docker
$ aws configure
```

![image](https://user-images.githubusercontent.com/41244373/180926651-e3702bc7-a51a-4ee2-a5d7-de0f8a641edd.png)

- `AWS Access Key ID`, `AWS Secret Access Key`에는 IAM에서 발급받은 값을 넣으면 된다. 그리고 `Default region name`에는 `ap-northeast-2`(서울)을 입력하면 된다.

# 4. ECR Repository 생성하기

![image](https://user-images.githubusercontent.com/41244373/180926676-2c475d90-0aa0-423a-b75a-cb16375140c7.png)

![image](https://user-images.githubusercontent.com/41244373/180926896-131b062c-f7bb-4a79-a787-2b5420815690.png)

![image](https://user-images.githubusercontent.com/41244373/180926928-5af19545-368c-452c-8eae-d5931a94d3ea.png)

# 5. ECR에 Docker Image Push 해보기

![image](https://user-images.githubusercontent.com/41244373/180926957-e6ce60e0-a3b8-49c1-9896-e6aed137e481.png)

# 6. Docker Image Pull 해보기

```docker
$ docker pull {Docker Image URI}
```

- Docker Image가 저장된 URI는 아래의 그림에서 확인할 수 있다.

  ![image](https://user-images.githubusercontent.com/41244373/180926988-99b80ca6-9668-44f2-b443-c0b5a2ac43a7.png)


# References

- [[AWS] AWS Elastic Container Registry 간단한 실습해보기](https://devlog-wjdrbs96.tistory.com/324)

- [ECR 사용하기](https://blog.illunex.com/2020-08-25/)