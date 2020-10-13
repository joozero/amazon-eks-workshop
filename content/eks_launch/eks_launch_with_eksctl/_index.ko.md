---
date: 2020-09-03
title: "eksctl로 클러스터 생성하기"
weight: 410
pre: "<b>5-1  </b>"
---

#### eksctl을 사용하여 EKS 클러스터 생성하기

eksctl을 사용하여 아무 설정 값을 주지 않고 아래의 명령어로 실행하면 default parameter로 클러스터가 배포됩니다.
```
eksctl create cluster
```
{{% notice info %}}
하지만 몇 가지 값을 커스텀하게 주기 위해 **구성 파일을 작성**하여 배포합니다. 뒤의 실습에서 쿠버네티스의 오브젝트를 생성할 때에도 단순히 kubectl CLI로 생성하는 것이 아닌 구성 파일을 작성하여 생성합니다. 이는 개인이 명시한 오브젝트들의 바라는 상태(desired state)를 쉽게 파악하고 관리할 수 있도록 하는 이점이 있습니다.
{{% /notice %}}

1. root 폴더(/home/ec2-user/environment) 위치에서 아래의 값을 붙여넣습니다.
    ```
    cat << EOF > eks-demo-cluster.yaml
    ---
    apiVersion: eksctl.io/v1alpha5
    kind: ClusterConfig

    metadata:
    name: eks-demo # 생성할 EKS 클러스터명
    region: ap-northeast-2 # 클러스터를 생성할 리젼
    version: "1.17"

    vpc:
    cidr: "192.168.0.0/16" # 클러스터에서 사용할 VPC의 CIDR

    managedNodeGroups:
    - name: eks-demo-node-group # 클러스터의 노드 그룹명
    instanceType: m5.large # 클러스터 워커 노드의 인스턴스 타입
    desiredCapacity: 3 # 클러스터 워커 노드의 갯수
    volumeSize: 10  # 클러스터 워커 노드의 EBS 용량 (단위: GiB)
    iam:
        withAddonPolicies:
        ImageBuilder: true # AWS ECR에 대한 권한 추가
        albIngress: true  # alb ingress에 대한 권한 추가

    cloudWatch:
    clusterLogging:
        enableTypes: ["*"]
    EOF
    ```
2. 아래의 명령어를 통해, 클러스터를 배포합니다.
   ```
    eksctl create cluster -f eks-demo-cluster.yaml
    ```
    클러스터가 완전히 배포되는데까지 약 15분 ~ 20분 정도 소요됩니다. AWS Cloud9 터미널 창에서 클러스터 배포 진행 사항을 파악할 수 있고, AWS CloudFormation 콘솔창에서도 이벤트 및 리소스 등의 상태를 파악할 수 있습니다. 
3. 배포가 완료되면 아래의 명령어를 통해, 노드가 제대로 배포되었는지 확인합니다.
    ```
    kubectl get nodes 
    ```
4. 또한, **~/.kube/config**에 클러스터 자격 증명이 더해진 것을 확인할 수 있습니다.

* * *

#### 현재의 아키텍처
eksctl로 클러스터를 생성한 후, 현재까지 구성된 서비스의 아키텍처는 아래와 같습니다.

![current architecture](/images/eks_launch/current-architecture.svg)