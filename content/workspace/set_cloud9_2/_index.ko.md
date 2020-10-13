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
    ```
    bash_profile에 해당 값을 넣습니다.
    ```
    echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
    ```
    ```
    aws configure set default.region ${AWS_REGION}
    ```
    설정한 리전 값을 확인합니다.
    ```
    aws configure get default.region
    ```