---
date: 2020-09-03
title: "AWS Cloud9 추가 셋팅하기"
weight: 280
pre: "<b>3-3  </b>"
---

앞선 페이지에서 **AWS Cloud9 IDE 구축** 및 **필요한 툴 설치**를 수행한 후, 아래의 추가 설정 작업을 진행합니다.

1. 현재 실습이 진행되고 있는 리전을 기본 값으로 하도록 aws cli를 설정합니다.
    ```
    export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')

    echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
    
    aws configure set default.region ${AWS_REGION}
    ```
    설정한 리전 값을 확인합니다.
    ```
    aws configure get default.region
    ```
2. 현재 실습을 진행하는 계정 ID를 환경 변수로 등록합니다.
    ```
    export ACCOUNT_ID=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.accountId')

    echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
    ```

{{% notice info %}}
리전을 명시해주지 않으면 클러스터를 배포한 후, 관련 정보를 확인할 때 에러가 뜹니다.
{{% /notice %}}


