---
date: 2020-09-03
title: "Cluster Autoscaler 적용하기"
weight: 720
pre: "<b>8-2  </b>"
---

#### Cluster Autoscaler를 사용하여 클러스터 스케일링 적용하기 
이전 페이지에서 파드에 오토 스케일링을 적용하였습니다. 하지만 트래픽에 따라 파드가 올라가는 클러스터의 자원이 모자라게 되는 경우도 발생하게 됩니다. 즉, 워커 노드가 가득 차서 파드가 스케줄될 수 없는 상태가 되는 것이죠. 이때, 사용하는 것이 **Cluster Autoscaler(CA)** 입니다.

![CA scaling](/images/eks_scaling/k8s-ca-scaling.svg)

Cluster Autoscaler(CA)는 pending 상태인 파드가 존재할 경우, 워커 노드를 스케일 아웃합니다. 특정 시간을 간격으로 사용률을 확인하여 스케일 인/아웃을 수행합니다. 그리고 AWS에서는 Auto Scaling Group을 사용하여 Cluster Autoscaler를 적용합니다.

![CA scaling situation](/images/eks_scaling/k8s-ca-scaling-view.png)

* * *

1. 아래의 명령어로 현재 클러스터의 워커노드에 적용된 ASG(Auto Scaling Group)의 값을 확인합니다.
    ```
    aws autoscaling \
        describe-auto-scaling-groups \
        --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eks-demo']].[AutoScalingGroupName, MinSize, MaxSize,DesiredCapacity]" \
        --output table
    ```
2. 그 다음 [IAM 정책 콘솔](https://console.aws.amazon.com/iam/home?#/policies)에 접속하여 **Cluster AutoScaler** 라는 이름을 가진 IAM 정책을 생성합니다.
    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "autoscaling:DescribeAutoScalingGroups",
                    "autoscaling:DescribeAutoScalingInstances",
                    "autoscaling:DescribeLaunchConfigurations",
                    "autoscaling:DescribeTags",
                    "autoscaling:SetDesiredCapacity",
                    "autoscaling:TerminateInstanceInAutoScalingGroup",
                    "ec2:DescribeLaunchTemplateVersions"
                ],
                "Resource": "*"
            }
        ]
    }
    ```
    해당 정책을 통해, 오토 스케일링 기능을 활성화할 수 있습니다.
    ![eks ca policy](/images/eks_scaling/eks-ca-policy.png)
3. [AWS CloudFormation 콘솔](http://console.aws.amazon.com/cloudformation)을 열고 노드 그룹 스택을 선택한 다음, **Resource** 탭을 선택합니다. **EKS 워커 노드에 연결된 인스턴스 역할**에 방금 생성한 정책을 연결합니다. 
    ![eks ca cloudformation](/images/eks_scaling/eks-ca-cloudformation.png)
4. Cloud9 환경으로 돌아와 Cluster Atuoscaler 프로젝트에서 제공하는 배포 예제 파일을 다운로드합니다.
    ```
    wget https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
    ```
5. 다운로드한 yaml 파일을 열고 클러스터 이름(eks-demo)을 설정한 후, 배포합니다.
   ```
    ...          
            command:
                - ./cluster-autoscaler
                - --v=4
                - --stderrthreshold=info
                - --cloud-provider=aws
                - --skip-nodes-with-local-storage=false
                - --expander=least-waste
                - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/eks-demo
    ...
   ```

   ```
   kubectl apply -f cluster-autoscaler-autodiscover.yaml
   ```
6. 오토스케일링 기능이 정상적으로 작동하는지 확인하기 위해 간단한 부하 테스트를 진행합니다. 워커 노드를 늘리기 위해, 100개의 파드를 배포하는 명령을 수행합니다.
   ```
   kubectl create deployment autoscaler-demo --image=nginx
   kubectl scale deployment autoscaler-demo --replicas=100
   ```
   파드의 배포 진행 상태를 파악하기 위해 아래의 명령어를 수행합니다.
   ```
   kubectl get deployment autoscaler-demo --watch
   ```
7. 워커 노드 수를 확인합니다.
   ```
    kubectl get nodes
   ```