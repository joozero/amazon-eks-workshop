---
date: 2020-07-29
title: "Amazon Forecast 사용해보기"
chapter: false
weight: 200
pre: "<b>3. </b>"
---

## 실습 소개
{{% notice info %}}
Amazon Forecast의 경우, **AWS 콘솔, AWS CLI, Jupyter(Python) 노트북**이라는 3가지 옵션 중 하나로 쉽게 시작할 수 있습니다.

본 실습은 AWS 콘솔 창에서 하나씩 클릭하는 방식이 아닌 **Jupyter(Python) 노트북을 사용하여 Amazon Forecast를 API로 사용하는 방법**에 대해 살펴봅니다. 여러분들이 가진 실제 데이터를 가지고 Amazon Forecast를 적용하기 위해서는 데이터 전처리와 같은 작업들이 필요합니다. 이와 같이 커스텀하게 작업하는 부분들을 소스 코드로 간편하게 작업하기 위해 Jupyter 노트북을 사용합니다.
{{% /notice %}}

## 전체 아키텍처
실습의 아키텍처는 간단합니다. 데이터를 담는 공간인 S3 버킷을 생성하고 Forecasting 작업을 수행할 SageMaker Notebook 인스턴스만 있으면 됩니다. 대부분의 작업은 Jupyter 노트북에서 수행됩니다.
![Forecast Architecture](/images/one-time-analytics/amazon_forecast_architecture.svg)