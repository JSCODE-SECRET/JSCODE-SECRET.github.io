---
layout: post
title: 'AWS EC2와 AWS Labmda의 비용 비교'
subtitle: 'AWS EC2와 AWS Labmda의 비용 비교'
categories: Infra
comments: false
---

# 비용

> 가정 : 메모리 1GB를 사용  

### AWS EC2 비용

![AWS EC2 비용](https://user-images.githubusercontent.com/41244373/180592160-7cf87866-13cf-44c7-a1ce-0a35c4dac988.png)

- **t2.micro 한 달 비용** : 0.0144 USD * (24시간 * 30일) = **10.368 USD**
- **t2.small 한 달 비용** : 0.0288 USD * (24시간 * 30일) = **20.736 USD**
- **t2.medium 한 달 비용** : 0.0576 USD * (24시간 * 30일) = **41.472USD**

### AWS Lambda 비용

![AWS Lambda 비용](https://user-images.githubusercontent.com/41244373/180592161-30ef0f53-2d56-42f5-a24a-4a9503cb2490.png)

- **한 달 비용 (= Lambda가 쉬지 않고 풀가동 했을 때를 가정)** : 0.0000000167 USD * (1000ms * 3600초 * 24시간 * 30일) = **43.2864 USD**

# 결론
실제로 Labmda는 사용한 만큼만 비용을 지불하기 때문에, 24시간 내내 사용하지 않고 일부 시간에 대해서만 사용하게 되거나 트래픽의 발생량이 지극히 적은 경우에는 EC2보다 Lambda를 사용하는 것이 비용적 이점이 많을 가능성이 높다.

# References

- [서버리스 컴퓨팅 - AWS Lambda 요금 - Amazon Web Services](https://aws.amazon.com/ko/lambda/pricing/)