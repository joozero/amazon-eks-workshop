---
date: 2020-09-03
title: "kubectl 설치"
weight: 240
pre: "<b>  </b>"
---

#### kubectl 설치하기
**kubectl**은 쿠버네티스 클러스터에 명령을 내리는 CLI입니다. 

쿠버네티스는 오브젝트 생성, 수정 혹은 삭제와 관련한 동작을 수행하기 위해 **쿠버네티스 API**를 사용합니다. 이때, kubectl CLI를 사용하면 해당 명령어가 쿠버네티스 API를 호출해 관련 동작을 수행합니다.

* * *

[여기](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)를 클릭하여 **배포할 Amazon EKS 버전과 상응하는 kubectl를 설치**합니다.
본 실습에서는 가장 최신의 kubectl 바이너리를 설치합니다(2020년 12월 기준 1.18).
```
sudo curl -o /usr/local/bin/kubectl \
   https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.9/2020-11-02/bin/linux/amd64/kubectl

sudo chmod +x /usr/local/bin/kubectl
```
아래의 명령어를 통해, 최신의 kubectl이 설치되었는지 확인합니다.
```
kubectl version --client=true --short=true

# 출력되는 결과 값
Client Version: v1.18.9-eks-d1db3c
```