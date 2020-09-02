---
date: 2020-07-29
title: "Amazon Forecast"
weight: 12
pre: "<b>   </b>"
---

## Amazon Forecast에 대하여
{{% notice info %}}
[Amazon Forecast](https://docs.aws.amazon.com/ko_kr/forecast/latest/dg/what-is-forecast.html)는 정확한 예측을 위해 기계 학습을 사용하는 완전관리형 서비스입니다. Amazon Forecast에 과거의 시계열 데이터를 제공하면 시계열에서 미래의 지점을 예상할 수 있습니다. 이러한 예측은 소매, 재무 계획 등 다양한 도메인에 유용하게 적용될 수 있습니다. 
{{% /notice %}}

앞선 페이지에서 시계열 예측은 무엇인지, 그리고 시계열 예측에서 정확도가 얼마나 중요한 부분을 차지하는지 간략하게 설명드렸습니다.
하지만 실제 환경에서 정확한 예측은 달성하기 힘든 과제 중 하나입니다. 과거의 시계열 데이터는 시간이라는 요인 외에도 날씨, 휴일, 이벤트 등의 외부 요인에 영향을 받기 때문입니다. 또한, 데이터 자체가 불규칙한 패턴을 가지고 있는 경우도 있고, 상품의 브랜드와 같은 제품의 속성에 영향을 받는 경우가 있습니다. 


Amazon Forecast를 사용하면 시계열 분석에서 사용하는 ARIMA 알고리즘, ETS 알고리즘, DeepAR+ 알고리즘 등이 미리 정의되어 있어 손쉽게 기계 학습 모델을 구축할 수 있을 뿐만 아니라 모델 교육을 위해 AutoML 옵션도 제공합니다. AutoML은 데이터 세트에 기반한 최적의 알고리즘 선택, 하이퍼파라미터 튜닝(HPO), 반복 모델링 및 모델 평가와 같은 복잡한 기계 학습 작업을 자동화합니다.
![Amazon Forecast](/images/overview/forecast_how_it_works.png)
또한, DeepAR+ 알고리즘을 사용하면 dataset에 과거의 시계열 데이터(대상 시계열, Target time-series dataset) 뿐만 아니라 관련 시계열(Relatd time-series dataset) 및 항목 메타데이터(Item metadata)를 포함하여 모델을 교육할 수 있습니다. 추가적인 데이터를 제공함으로써 단순히 시계열 데이터만 확인하는 방식에 비해 최대 50% 더 정확히 예측하는 모델을 생성할 수 있습니다.


Amazon Forecast는 완전관리형 서비스이므로 서버를 프로비저닝하거나 기계 학습 모델을 구축, 교육 또는 배포할 필요가 없습니다. 사용한 만큼만 비용을 지불하면 되고, 최소 요금 및 사전 약정은 없습니다.



