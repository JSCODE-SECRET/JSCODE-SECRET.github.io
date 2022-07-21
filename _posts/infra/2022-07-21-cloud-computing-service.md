---
layout: post
title: '클라우드 컴퓨팅 서비스란? / IaaS, SaaS, PaaS, FaaS'
subtitle: '클라우드 컴퓨팅 서비스란? / IaaS, SaaS, PaaS, FaaS'
categories: Infra
comments: false
---
![IaaS, SaaS, PaaS, FaaS 이미지](https://user-images.githubusercontent.com/41244373/180115542-1efddf8a-fd2e-4a5c-bd9e-85148c1308bb.png)

최근에 대부분의 사람들이 클라우드 컴퓨팅 서비스(ex. AWS, GCP, Azure 등)을 활용한다. 클라우드 컴퓨팅 서비스에서는 여러가지 형태의 서비스를 제공한다.


### 1. IaaS(Infrastructure as a Service)

기본 인프라(가상 머신, 소프트웨어 정의 네트워크, 연결된 스토리지)만 제공합니다. AWS EC2로 가정하면 가상 머신은 Ubuntu t2.micro와 같은 것을 의미하고, 소프트웨어 정의 네트워크는 VPC, 서브넷, 보안그룹 등을 설정할 수 있는 부분을 의미하고, 연결된 스토리지는 EBS를 의미한다. IaaS에서의 최종 사용자는 플랫폼과 환경을 구성 및 관리하고 여기에 애플리케이션을 배포해야 합니다.

- 예시 : AWS EC2
- 맨 위의 그림에 따르면, IaaS는 Virtualization, Servers, Storage, Networking까지만 제공을 해주고, 나머지에 대해서는 제공하지 않는 것을 의미한다.

### 2. SaaS (Software as a Service)

Application, Data, Runtime, Middleware, O/S, Virtualization, Servers, Storage, Networking을 통째로 제공하는 형태이다. 이로 인해 이 서비스를 사용하는 사람은 개발에 대한 부분을 이해할 필요도 없고 코드적으로 건드릴 부분 없이 해당 서비스를 바로 사용 하면 된다.

GMAIL은 SaaS의 가장 좋은 예시이다. 브라우저를 통해 GMAIL을 사용하기 위해 필요한 모든 것을 Google 팀에서 관리한다.

### 3. PaaS(Platform as a Service)

인프라 구축 및 유지 관리의 복잡성 없이 최종 사용자가 애플리케이션을 개발, 실행 및 관리할 수 있는 플랫폼을 제공한다.

- 예시 : AWS ElasticBeanstalk

### 4. FaaS(Function as a Service)

인프라 구축 및 유지 관리의 복잡성(=서버 관리) 없이 고객이 애플리케이션 기능을 개발, 실행 및 관리할 수 있는 플랫폼을 제공한다. 특정 이벤트에 반응하는 함수를 등록하고 해당 이벤트가 발생하면 함수가 실행되는 구조이다. 서버 관리를 할 필요가 없다보니 Serveless라고 부르기도 함.

FaaS는 프로젝트를 여러개의 함수로 쪼개서 (혹은 한 개의 함수로 만들어서), 매우 거대하고 분산된 컴퓨팅 자원에 여러분이 준비해둔 함수를 등록하고, 이 함수들이 실행되는 횟수 (그리고 실행된 시간)만큼 비용을 내는 방식을 말한다.

- 예시 : AWS Lambda

# References
- [What are Cloud Computing Services [IaaS, CaaS, PaaS, FaaS, SaaS]](https://medium.com/@nnilesh7756/what-are-cloud-computing-services-iaas-caas-paas-faas-saas-ac0f6022d36e)
- [클라우드의 특징과 종류(IaaS, PaaS, SaaS, FaaS)](https://yechoi.tistory.com/64)