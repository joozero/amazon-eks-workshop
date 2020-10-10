---
date: 2020-09-03
title: "ALB 인그레스 컨트롤러 만들기"
weight: 510
pre: "<b>6-1  </b>"
---

#### ALB 인그레스 컨트롤러 만들기
Amazon EKS의 ALB 인그레스 컨트롤러란 

* * *

먼저, 앞으로의 매니페스트를 관리하기 위해 루트 폴더(예: /home/ec2-user/environment/)에서 **manifests**라는 이름을 가진 폴더를 생성합니다. 그 후, manifests 폴더 안에서, ALB 인그레스 컨트롤러 관련 매니페스트를 관리하기 위한 폴더 **alb-ingress-controller**를 만듭니다.
```
mkdir -p manifests/alb-ingress-controller

# 최종 폴더 위치
/home/ec2-user/environment/manifests/alb-ingress-controller
```

1. ALB 인그레스 컨트롤러 매니페스트를 다운 받습니다.
    ```
    wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/alb-ingress-controller.yaml
    ```
2. 매니페스트 파일에서 --cluster-name=devCluster 값을 [5-1에서 생성한 클러스터 이름](https://master.d3s71i2n51x60t.amplifyapp.com/ko/ingress_controller_launch/)으로 변경한 후, 저장합니다. 본 실습에서는 **eks-demo-cluster**로 설정합니다.
    ```
    --cluster-name=eks-demo-cluster
    ```
3. RBAC roles 매니페스트를 배포합니다.
    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/rbac-role.yaml
    ```
4. 2번에서 수정한 ALB 인그레스 컨트롤러 매니페스트 파일을 배포합니다.
    ```
    kubectl apply -f alb-ingress-controller.yaml
    ```
5. 배포가 성공적으로 되고 컨트롤러가 실행되는지 아래의 명령어를 통해 확인합니다.
    ```
    kubectl logs -n kube-system $(kubectl get po -n kube-system | egrep -o "alb-ingress[a-zA-Z0-9-]+")
    ```