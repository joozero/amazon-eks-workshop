---
date: 2020-07-29
title: "실습 환경 구축"
chapter: false
weight: 210
pre: "<b>3-1 </b>"
---

### 실습 환경 구축하기
> 본 페이지에서는 데이터를 저장할 S3 버킷을 생성하고, Jupyter 노트북을 사용하기 위해 SageMaker Notebook 인스턴스를 생성하는 작업을 안내합니다.

{{% notice warning %}}
본 실습의 경우, **ap-northeast-2(서울) 리전**에서 수행합니다.
{{% /notice %}}


* * *
### S3 버킷 생성하기

Amazon Forecast에서 사용하는 모든 데이터는 오브젝트 스토리지인 Amazon S3에 저장되어야 합니다. Amazon S3에 데이터를 저장하기 전에 반드시 Bucket을 생성해야 합니다.


1. [S3 콘솔](https://console.aws.amazon.com/s3/home)에 접속합니다. 이때 아래 화면과 같지 않다면
{{% notice note %}}
We've temporarily re-enabled the previous version of the S3 console while we continue to improve the new S3 console experience. Switch to the new console.
{{% /notice %}}
상단의 메세지에서 **Switch to the new console**(새로운 S3 콘솔 환경) 링크 부분을 클릭하여 화면 전환을 수행합니다.
![S3 Bucket Create](/images/preparation/s3_create.png)
2. **Create Bucket**(버킷 만들기) 버튼을 눌러 버킷을 생성합니다.
3. **Bucket name** 필드에 고유한 버킷 이름을 입력합니다. 본 실습에서는 `forecast-bucket-사용자이름`으로 입력하여 진행합니다. **입력한 버킷 이름은 Amazon S3내에서 중복될 수 없고 유일**해야 합니다. 조직 이름 또는 사용자 이름 등을 반영하여 유일한 버킷 이름을 선택합니다. **Region** 드롭다운 박스에서 버킷을 생성할 리전을 지정합니다. 본 실습에서는 **Asia Pacific (Seoul) ap-northeast-2**을 선택합니다. Bucket settings for Block Public Access는 기본 값을 사용합니다. 마지막으로 우측 하단의 Create bucket를 선택합니다.
![S3 Bucket Create](/images/preparation/s3_create_02.png)
{{% notice tip %}}
버킷이 생성되는 리전에 따라 추가적인 제약이 있을 수 있습니다. 버킷의 이름은 한 번 생성하면 변경이 불가능하며, 버킷 내에 저장된 오브젝트를 지정하기 위하여 URL에 포함됩니다. 생성할 버킷의 이름이 적절한지 확인하시기 바랍니다.
{{% /notice %}}
4. S3 버킷이 생성되었습니다.
![S3 Bucket Create](/images/preparation/s3_create_03.png)
{{% notice tip %}}
버킷을 생성하는 것만으로는 비용이 과금 되지 않습니다. 버킷에 오브젝트를 저장하거나 오브젝트를 송수신하는 것에 대해서만 과금됩니다.
{{% /notice %}}


* * *
### SageMaker Notebook 인스턴스 생성하기

1. [SageMaker 콘솔](https://console.aws.amazon.com/sagemaker/home)에 접속한 후, 화면 왼쪽 사이드 바에서 **Notebook instances**(노트북 인스턴스)를 클릭합니다.
![Notebook Instance Create](/images/preparation/notebook_create.png)
2. **Create notebook instance**(노트북 인스턴스 생성) 버튼을 눌러 인스턴스를 생성합니다.
3. **Notebook instance name** 필드에 노트북 인스턴스 이름을 입력합니다. 본 실습에서는 `	forecast-demogo-notebook`으로 입력하여 진행합니다. **Notebook instance type** 드롭다운 박스에서 적합한 인스턴스 타입을 선택합니다. 본 실습에서는 **ml.t2.medium**을 선택합니다.
![Notebook Instance Create](/images/preparation/notebook_create_02.png)
4. **IAM role** 드롭다운 박스에서 **create a new role**을 선택하면 아래와 같은 팝업 창이 생깁니다. **S3 buckets you specify - _optional_** 아래 **Specific S3 Buckets**를 선택합니다. 그리고 텍스트 필드에 앞서 생성한 S3 버킷 이름(`forecast-bucket-사용자이름`)을 입력합니다. 이후, **Create role**을 클릭합니다.
![Notebook Instance Create](/images/preparation/notebook_create_03.png)
5. IAM role이 생성된 후, 오른쪽 하단의 **Create notebook instance** 버튼을 눌러 노트북 인스턴스를 생성합니다. 해당 인스턴스가 생성되기까지는 약 5분 정도의 시간이 소요됩니다. 생성 완료되면 **Status** 칼럼 부분이 InService로 변경된 것을 확인할 수 있습니다.
{{% notice warning %}}
본 실습에서는 노트북 인스턴스의 **IAM role**에 추가적인 권한을 부여해야 가능한 작업들이 있습니다. 따라서 아래의 단계도 이어서 수행합니다.
{{% /notice %}}
6. 방금 생성한 노트북 인스턴스를 클릭한 후, 스크롤을 내리면 **Permissions and encryption**(권한 및 암호화) 필드가 있습니다. 해당 필드에서 **IAm role ARN**을 클릭하여 IAM 콘솔창으로 이동합니다.
![IAM Policy Add](/images/preparation/iam_policy_add.png)
7. 해당 IAM role에서 **Attach policies** 버튼을 클릭합니다.
![IAM Policy Add](/images/preparation/iam_policy_add_02.png)
8. **Filter policies** 검색바에서 `AmazonForecastFullAccess`와 `IAMFullAccess`를 조회하여 체크합니다. 그리고 오른쪽 하단에 **Attach policy**를 클릭합니다.

![IAM Policy Add](/images/preparation/iam_policy_add_03.png)

![IAM Policy Add](/images/preparation/iam_policy_add_04.png)

9.  해당 IAM role에 두 개의 추가 policy가 적용되었는지 확인합니다.
![IAM Policy Add](/images/preparation/iam_policy_add_05.png)

{{% notice note %}}
실습을 진행하기 위한 모든 준비가 끝났습니다. 다음 페이지로 이동하여 본격적인 실습을 수행합니다.
{{% /notice %}}