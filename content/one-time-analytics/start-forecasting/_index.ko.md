---
date: 2020-07-29
title: "노트북에서 Forecast 사용하기"
chapter: false
weight: 220
pre: "<b>3-2 </b>"
---

### 실습 순서
Jupyter 노트북에서 수행할 실습 단계는 아래와 같습니다.

1. 필요한 Python 라이브러리 준비
2. 데이터 전처리 
3. Forecast 구성
      1. Data Group(데이터 그룹) 생성
      2. 대상 시계열 Dataset(데이터셋) 생성
      3. 대상 시계열 데이터 넣기
      4. Predictor(예측기) 훈련 및 생성
      5. Forecast(예측) 생성
4. Forecast 활용하기
{{% notice info %}}
3번과 4번은 아래의 AWS 콘솔에서 Amazon Forecast 서비스를 이용할 때, 나타나는 대시보드 순서와 맵핑됩니다.
{{% /notice %}}
![Forecast Dashboard](/images/one-time-analytics/forecast_dashboard.png)


* * *

### 실습 목표
본 실습에서는 MLOps pipeline 구성에 앞서, **데이터 전처리 및 적절한 Forecast Hyper Parameter 등을 확인하기 위한 PoC 과정**을 수행합니다.


* * *
### S3에 데이터 넣기
1. [S3 콘솔](https://console.aws.amazon.com/s3/home)에 접속한 후, 3-1 실습 환경 구축에서 생성한 S3 버킷을 클릭합니다.
![S3 Bucket](/images/one-time-analytics/s3_bucket.png)
2. **Create folder**를 클릭하여 **raw_data**라는 폴더를 생성합니다.  
![S3 Bucket](/images/one-time-analytics/s3_bucket_02.png)
3. 같은 단계를 거쳐 **poc**와 **forecast_data**라는 이름을 가진 폴더도 생성합니다. 3개의 폴더를 생성한 후, 기대할 수 있는 결과는 아래와 같습니다. 각 폴더의 사용 용도에 대해서는 실습을 수행하는 동안 설명합니다.
![S3 Bucket](/images/one-time-analytics/s3_bucket_03.png)
4. 아래의 링크로 접속해서 데이터를 다운로드 받습니다.
```
https://github.com/demogo-prime/forecast-for-ecommerce-data
```
![S3 Bucket](/images/one-time-analytics/s3_bucket_04.png)
5. 다운받은 데이터(파일명: forecast_data.csv)를 **raw_data** 폴더 안에 업로드합니다.
![S3 Bucket](/images/one-time-analytics/s3_bucket_05.png)


* * *
### JupyterLab 시작하기
1. [SageMaker 콘솔](https://console.aws.amazon.com/sagemaker/home)에 접속한 후, 화면 왼쪽 사이드 바에서 **Notebook instances**(노트북 인스턴스)를 클릭합니다. 3-1 실습 환경 구축에서 생성한 노트북 인스턴스의 Actions 칼럼 부분에서 **Open JupyterLab**을 클릭합니다.
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook.png)
2.  클릭하면 노트북 인스턴스의 JupyterLab 페이지로 이동하게 됩니다. 상단의 **Git** 메뉴에서 **Clone** 항목을 클릭합니다. 아래의 URL을 붙인 후, **CLONE** 버튼을 누릅니다.
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_02.png)

* * *
### 코드 실행하기
{{% notice note %}}
앞선 작업을 통해 노트북 파일을 다운로드 받으면 아래와 같은 화면이 나타납니다. 
코드가 작성된 부분에서 **shfit + enter**를 누르면 왼쪽 상태가 **In [  * ]** 로 변경되면서 해당 코드 블록이 실행된 후, 다음 코드 블록으로 이동합니다. (**ctrl + enter**를 누를 경우, 다음 코드 블록 이동 없이 해당 코드 블록만 실행합니다.)
{{% /notice %}}
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_03.png)

####  참고 사항 
-  노트북 파일에는 각 단계마다 구간을 나눈 형태로 제공됩니다. 코드 블록을 하나씩 실행시키면서 **그에 따른 결과 값도 즉각적으로 확인**할 수 있습니다.
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_04.png)
-  노트북 파일에 있는 주석 내용을 참고하시면서 실습을 진행합니다. 예를 들어, 아래의 코드 블록을 실행할 때에는 **생성한 버킷명으로 이름을 바꿔야함**을 명시한 것입니다.
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_05.png)
-  Amazon Forecast 구성과 관련한 코드 블록을 단계마다 실행하시면 AWS Forecast 콘솔 창에서 실행된 결과를 확인할 수 있습니다. 아래 화면과 같이 노트북 파일에서 코드를 실행시키면
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_06.png)
그와 동시에 AWS Forecast 콘솔 창에서 결과를 확인할 수 있습니다.
![Jupyter Notebook](/images/one-time-analytics/jupyter_notebook_07.png)