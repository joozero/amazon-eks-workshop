---
date: 2020-08-11
title: "MLOps 파이프라인 구축"
chapter: false
weight: 220
pre: "<b>4-2 </b>"
---

### Sample 아키텍처 배포하기
Cloud9 IDE terminal에서 다음의 명령을 실행하여 샘플 소스 코드를 가져옵니다.
``` bash
git clone https://github.com/aws-samples/amazon-forecast-samples.git
```
코드 다운로드가 완료되면 **amazon-forecast-samples/ml_ops/visualization_blog/template.yaml** 파일을 열어 봅니다.

![MLOps yaml file](/images/mlops/mlops_yaml.png)
이 소스 코드와 아키텍처는 [MLOps Sample repo](https://github.com/aws-samples/amazon-forecast-samples/tree/master/ml_ops/visualization_blog)에서도 확인 할 수 있습니다.

해당 파일을 본 실습에 사용할 수 있도록 몇가지 내용들을 수정하여야 합니다.

Cloud9 IDE 메뉴 중 **Find->Replace** 를 선택 합니다.
![python version replace](/images/mlops/replace.png)

코드 하단에 생긴 replace field에 **Python3.7**을 **python3.6** 으로 변경 하게끔 입력하고 **Replace All** 을 클릭하여 수정 합니다.
![python version replace](/images/mlops/replace_python_version.png)

Cloud9 터미널에서 다음 명령어를 실행하여 yaml 파일의 위치로 이동 후 솔루션을 배포 합니다.
``` bash
cd amazon-forecast-samples/ml_ops/visualization_blog
sam build && sam deploy --guided
```
Build가 성공하면 배포를 위한 parameter를 아래와 같이 입력합니다. 본 실습은 **서울리전 ap-northeast-2**를 사용 합니다.

email 란에는 본인의 email을 입력합니다.
![SAM config](/images/mlops/sam_config.png)