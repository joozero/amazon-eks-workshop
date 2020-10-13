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
본 실습에서는 가장 최신의 kubectl 바이너리를 설치합니다(2020년 10월 기준 1.17).
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/amd64/kubectl
```
kubectl 바이너리를 실행 가능하게 변경해줍니다.
```
chmod +x ./kubectl
```
사용자가 명령어를 사용할 수 있도록 디렉토리에 옮겨줍니다.
```
sudo mv ./kubectl /usr/local/bin/kubectl
```
아래의 명령어를 통해, 최신의 kubectl이 설치되었는지 확인합니다.
```
kubectl version --client=true --short=true
```
아래와 같은 결과 값이 출력되는 것을 확인할 수 있습니다.
```
Client Version: v1.17.9-eks-4c6976
```