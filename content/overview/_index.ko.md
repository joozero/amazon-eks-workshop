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


#### Amazon S3
Amazon Simple Storage Service(Amazon S3)는 업계 최고의 확장성과 데이터 가용성 및 보안과 성능을 제공하는 객체 스토리지 서비스입니다. 즉, 어떤 규모 어떤 산업의 고객이든 이 서비스를 사용하여 웹 사이트, 모바일 애플리케이션, 백업 및 복원, 아카이브, 엔터프라이즈 애플리케이션, IoT 디바이스, 빅 데이터 분석 등과 같은 다양한 사용 사례에서 원하는 만큼의 데이터를 저장하고 보호할 수 있습니다.

[Amazon S3](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/Welcome.html)는 사용하기 쉬운 관리 기능을 제공하므로 특정 비즈니스, 조직 및 규정 준수 요구 사항에 따라 데이터를 조직화하고 세부적인 액세스 제어를 구성할 수 있습니다. Amazon S3는 99.999999999%의 내구성을 제공하도록 설계되었으며, 전 세계 기업의 수백만 애플리케이션을 위한 데이터를 저장합니다.
![Amazon S3 Feature](/images/overview/s3_function.gif)

본 실습에서는  예측에 사용할 dataset을 저장하기 위해 S3 버킷을 생성합니다.


#### Amazon Glue
AWS Glue는 고객이 분석을 위해 손쉽게 데이터를 준비하고 로드할 수 있게 지원하는 완전관리형 ETL(추출, 변환 및 로드) 서비스입니다. AWS Management Console에서 클릭 몇 번으로 ETL 작업을 생성하고 실행할 수 있습니다. AWS Glue가 AWS에 저장된 데이터를 가리키도록 하기만 하면, AWS Glue에서 데이터를 검색하고 관련 메타데이터(예: 테이블 정의, 스키마)를 AWS Glue 데이터 카탈로그에 저장합니다. 카탈로그에 저장되면, 데이터는 즉시 검색하고 쿼리하고 ETL에서 사용할 수 있는 상태가 됩니다.
![Amazon Glue](/images/overview/glue_with_various_data_store.png)
본 실습에서는 원본 데이터를 가공하기 위해 Glue의 [Crawler](https://docs.aws.amazon.com/ko_kr/glue/latest/dg/add-crawler.html) 및 [Job](https://docs.aws.amazon.com/ko_kr/glue/latest/dg/author-job.html) 기능을 사용합니다.


#### AWS Step Functions
[AWS Step Functions](https://docs.aws.amazon.com/ko_kr/step-functions/latest/dg/welcome.html)를 사용하면 여러 AWS 서비스를 서버리스 워크플로로 조정하여 앱을 신속하게 빌드 및 업데이트할 수 있습니다. 또한 AWS Lambda, AWS Fargate 및 Amazon SageMaker와 같은 서비스를 기능이 풍부한 애플리케이션에 하나로 결합하는 워크플로를 설계하고 실행할 수 있습니다. 워크플로는 일련의 단계로 이루어져 있으며, 한 단계의 출력이 다음 단계의 입력이 됩니다.
![AWS Step Functions](/images/overview/step_functions_how_it_works.png)
Step Functions를 사용하면 워크플로가 이해하기 쉽고 다른 사람에게 설명하기 쉬우며 변경하기 쉬운 상태 시스템 다이어그램으로 변환되므로 애플리케이션 개발을 훨씬 쉽고 직관적으로 수행할 수 있습니다. Step Functions가 자동으로 각 단계를 트리거 및 추적하고 오류가 발생할 경우 재시도하므로 애플리케이션이 의도한 대로 올바르게 실행됩니다. Step Functions를 사용하면 기계 학습 모델 교육, 보고서 생성, IT 자동화와 같이 장기 실행되는 워크플로를 만들 수 있습니다. 또한, IoT 데이터 수집, 스트리밍 데이터 처리와 같이 단기간에 대량을 처리하는 워크플로도 빌드할 수 있습니다.

본 실습에서는 MLOps 파이프라인을 구축하기 위해 Step Functions를 사용합니다.


#### Amazon QuickSight
[Amazon QuickSight](https://docs.aws.amazon.com/ko_kr/quicksight/latest/user/welcome.html)는 조직 내 모든 구성원에게 통찰력을 손쉽게 제공할 수 있게 지원하는 빠른 클라우드 기반 비즈니스 인텔리전스 서비스입니다. 완전관리형 서비스인 QuickSight를 사용하면 ML Insights가 포함된 대화형 대시보드를 손쉽게 생성 및 게시할 수 있습니다. 그러면 대시보드를 어떤 디바이스에서든 액세스할 수 있는 것은 물론, 애플리케이션, 포털, 웹 사이트에 임베딩할 수도 있습니다.
![Amazon QuickSight](/images/overview/quicksight_how_it_works.png)