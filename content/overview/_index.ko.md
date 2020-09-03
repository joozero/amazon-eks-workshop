---
date: 2020-07-29
title: "워크샵 소개"
weight: 10
pre: "<b>1. </b>"
---

## 워크샵 시작하기

본 워크샵은 크게 두 가지 파트로 나누어져 있습니다.

- [첫 번째 실습](https://master.dsd6loerysixy.amplifyapp.com/ko/one-time-analytics/)에서는 **AWS API를 통하여 Amazon Forecast 서비스**를 사용합니다. Amazon SageMaker Jupyter Notebook을 생성한 후, Forecast 구성하고 예측 값을 도출하는 방법을 이해하는데 초점을 두고 있습니다. 해당 실습을 통해, AWS 내에서 시계열 데이터 예측을 어떻게 수행할 수 있는지 살펴 볼 것입니다.

- [두 번째 실습](https://master.dsd6loerysixy.amplifyapp.com/ko/multi-time-analytics/)에서는 **MLOps 파이프라인**을 구성하여 첫 번째 실습에서 일회성으로 수행한 시계열 데이터 예측 프로세스를  반복적으로 사용할 수 있는 방법에 대해 살펴 볼 것입니다. 새로운 데이터가 들어오면 프로세스가 동작할 수 있도록 구축하는 방법에 대해 배울 것입니다.


* * *
## 전체 아키텍처
아래의 도표는 각 실습의 end-to-end 시스템 아키텍처를 보여줍니다. 두 가지 종류의 실습을 통해, Amazon Forecast를 사용하는 방법과 시계열 데이터 예측 프로세스를 자동화하는 방법에 대해 알아봅니다.

* * *
 ## AWS ML STACK 
> AWS에서 제공되는 다양한 AI/ML 서비스를 통해, 사용자는 기계 학습을 손쉽게 활용할 수 있습니다.
> 사용자는 기계 학습에 최적화된 인프라를 사용하는 방법부터 AWS에서 제공하는 AI 서비스를 사용하는 방법까지 니즈에 맞게 선택할 수 있습니다.
> 
> 참고 : [AWS 기계 학습 소개 페이지](https://aws.amazon.com/ko/machine-learning/ "AWS에서의 기계 학습")

![AWS ML STACK](/images/overview/aws_ml_stack.png)

* * *
## 워크샵에 사용할 주요 서비스
아래에 소개되는 각 서비스들은 오늘 실습에서 사용되는 AWS 서비스들입니다.

#### Amazon SageMaker
Amazon SageMaker는 모든 개발자 및 데이터 과학자가 기계 학습(ML) 모델을 빠르게 구축, 훈련 및 배포할 수 있도록 하는 완전 관리형 서비스입니다. SageMaker는 기계 학습 프로세스의 각 단계에서 부담스러운 작업을 제거하여 고품질의 모델을 보다 쉽게 개발할 수 있도록 합니다.
![Amazon SageMaker Overview](/images/overview/sagemaker_overview.png)

본 실습에서는 [Amazon SageMaker Notebook](https://docs.aws.amazon.com/ko_kr/sagemaker/latest/dg/nbi.html)을 사용합니다. 이 노트북 인스턴스를 사용하여 데이터를 준비 및 처리하고 기계 학습 모델을 훈련 및 배포하는 데 사용할 수 있는 Jupyter 노트북을 생성하고 관리할 수 있습니다. 
