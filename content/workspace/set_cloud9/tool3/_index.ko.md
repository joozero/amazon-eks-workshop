---
date: 2020-09-03
title: "aws-iam-authenticator 설치"
weight: 250
pre: "<b>  </b>"
---

#### aws-iam-authenticator 설치하기
Amazon EKS는 kubernetes cluster 인증(authentication)을 위해, IAM을 사용합니다. 이를 위해, AWS IAM authenticator를 설치하고, 인증을 사용하기 위해 kubectl configuration을 변경합니다. 

{{% notice warning %}}
만약 AWS CLI 버전이 1.16.156 >= version인 경우, authenticator를 설치할 필요가 없습니다. 대신 **aws eks get-token** 명령어를 사용합니다. 
{{% /notice %}}

* * *

[여기](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)를 클릭하여 aws-iam-authenticator를 설치합니다. AWS Cloud9의 경우, 리눅스 관련 가이드를 따라갑니다.

아래의 명령어를 통해, 정상적으로 설치되었는지 확인합니다.
```
aws-iam-authenticator help
```
출력 값은 아래와 같습니다.
```
beta_zero:~/environment $ aws-iam-authenticator help
A tool to authenticate to Kubernetes using AWS IAM credentials

Usage:
  aws-iam-authenticator [command]

Available Commands:
  help        Help about any command
  init        Pre-generate certificate, private key, and kubeconfig files for the server.
  server      Run a webhook validation server suitable that validates tokens using AWS IAM
  token       Authenticate using AWS IAM and get token for Kubernetes
  verify      Verify a token for debugging purpose
  version     Version will output the current build information
```