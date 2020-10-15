---
date: 2020-09-03
title: "EKS 클러스터 생성하기"
weight: 400
pre: "<b>5.  </b>"
---

Amazon EKS 클러스터는 다양한 방식으로 배포될 수 있습니다.

- [AWS 콘솔 창](https://console.aws.amazon.com/eks/home#/)에서 클릭으로 배포하는 방법
- [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) 혹은 [AWS CDK](https://docs.aws.amazon.com/cdk/api/latest/)와 같은 IaC(Infrastructure as Code) 도구를 사용해 배포하는 방법
- EKS의 공식 CLI인 [eksctl](https://eksctl.io/)로 배포하는 방법
- Terraform, Pulumi, Rancher 등으로 배포하는 방법

![eks cluster deploy method](/images/eks_launch/deploy_eks_cluster.png)

본 실습에서는 **eksctl**을 사용하여 EKS 클러스터를 생성합니다.