---
title: "실습 리소스 정리"
weight: 1000
pre: "<b>   </b>"
---

{{% notice warning %}}
이 실습을 마치면 사용한 AWS 계정에 비용이 추가로 발생하지 않도록 사용한 리소스를 삭제해야 합니다.
{{% /notice %}}

1. 인그레스 자원들을 삭제합니다. 이 때, 해당 yaml 파일이 위치한 폴더(/home/ec2-user/environment/manifests)에서 아래의 명령어를 수행합니다.
    ```
    kubectl delete -f ingress.yaml
    kubectl delete -f frontend-ingress.yaml
    kubectl delete -f alb-ingress-controller/alb-ingress-controller.yaml
    ```
2. 워크샵을 위해 생성한 eks 클러스터를 삭제합니다. 이때, [8-2 Cluster AutoScaler 적용하기](../../eks_scaling/scale_by_autoscaler)에서 워커 노드에 붙여준 IAM 정책을 제거합니다.
    ```
    eksctl delete cluster --name=eks-demo
    ```
    [!] AWS CloudFormation 콘솔에서 관련 스택이 모두 삭제된 것을 확인합니다.
3. 생성한 ECR 저장소를 삭제합니다.
    아래의 명령어를 통해, 생성한 저장소 리스트를 불러옵니다.
   ```
    aws ecr describe-repositories
   ```
   ```
   aws ecr delete-repository --repository-name demo-flask-backend --force
   aws ecr delete-repository --repository-name demo-nodejs-backend --force
   aws ecr delete-repository --repository-name demo-frontend --force
   ```
    
4. 생성한 Cloud9 IDE 환경을 삭제합니다.
