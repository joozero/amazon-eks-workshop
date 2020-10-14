---
date: 2020-09-03
title: "ALB 인그레스 컨트롤러 만들기"
weight: 510
pre: "<b>6-1  </b>"
---

#### ALB 인그레스 컨트롤러 만들기
[Amazon EKS의 ALB 인그레스 컨트롤러](https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html)란 클러스터에 인그레스 자원이 생성될 때에 ALB(Application Load Balancer) 및 필요한 자원이 생성되도록 트리거하는 컨트롤러입니다. 인그레스 자원들은 ALB를 구성하여 HTTP 또는 HTTPS 트래픽을 클러스터 내 파드로 라우팅합니다.

ALB 인그레스 컨트롤러에서 지원하는 **트래픽 모드**는 아래의 두 가지입니다.

- Instance(default): 클러스터 내 노드를 ALB의 대상으로 등록합니다. ALB에 도달하는 트래픽은 NodePort로 라우팅된 다음 파드로 프록시됩니다.
- IP: 파드를 ALB 대상으로 등록합니다. ALB에 도달하는 트래픽은 파드로 직접 라우팅됩니다. 해당 트래픽 모드를 사용하기 위해선 ingress.yaml 파일에 주석을 사용하여 명시적으로 지정해야 합니다.

![](/images/ingress_controller_launch/alb-ingress-controller-traffic-mode.svg)

* * *
{{% notice note %}}
먼저, 앞으로의 매니페스트를 관리하기 위해 루트 폴더(예: /home/ec2-user/environment/)에서 **manifests**라는 이름을 가진 폴더를 생성합니다. 그 후, manifests 폴더 안에서, ALB 인그레스 컨트롤러 관련 매니페스트를 관리하기 위한 폴더 **alb-ingress-controller**를 만듭니다.
{{% /notice %}}

```
mkdir -p manifests/alb-ingress-controller

# 최종 폴더 위치
/home/ec2-user/environment/manifests/alb-ingress-controller
```

1. ALB 인그레스 컨트롤러 매니페스트를 다운 받습니다.
    ```
    wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/alb-ingress-controller.yaml
    ```
2. 매니페스트 파일에서 --cluster-name=devCluster 값을 [5-1에서 생성한 클러스터 이름](https://master.d3s71i2n51x60t.amplifyapp.com/ko/ingress_controller_launch/)으로 변경한 후, 저장합니다. 본 실습에서는 **eks-demo**로 설정합니다.
    ```
    --cluster-name=eks-demo
    ```
3. 추가적으로 --aws-vpc-id=vpc-xxxxxx 값과 ---aws-region=us-west-1도 수정합니다. aws-vpc-id의 경우, 아래의 cli로 도출된 결과 값을 넣습니다.
    ```
    eksctl get cluster --name eks-demo
    ```

    ```
    --aws-vpc-id=vpc-{your vpc id}
    --aws-region=ap-northeast-2
    ```

수정한 결과로 도출되는 매니페스트(alb-ingress-controller.yaml)는 아래와 같습니다

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/name: alb-ingress-controller
  name: alb-ingress-controller
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: alb-ingress-controller
  template:
    metadata:
      labels:
        app.kubernetes.io/name: alb-ingress-controller
    spec:
      containers:
        - name: alb-ingress-controller
          args:
            - --ingress-class=alb
            - --cluster-name=eks-demo
            - --aws-vpc-id=vpc-{your vpc id}
            - --aws-region=ap-northeast-2
          env:
          image: docker.io/amazon/aws-alb-ingress-controller:v1.1.8
      serviceAccountName: alb-ingress-controller
```

4. AWS ALB Ingress Controller를 배포하기 전, IAM OIDC 
5. RBAC roles 매니페스트를 배포합니다. 쿠버네티스 RBAC(Role-based access control)이란 역할 기반 권한 관리입니다. 
    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/rbac-role.yaml
    ```
    [!] 해당 매니페스트에는 클러스터롤과 클러스터롤 바인딩 그리고 서비스 어카운트가 기재되어 있습니다.
6. 2번에서 **수정한 ALB 인그레스 컨트롤러 매니페스트 파일** 을 배포합니다.
    ```
    kubectl apply -f alb-ingress-controller.yaml
    ```
7. 배포가 성공적으로 되고 컨트롤러가 실행되는지 아래의 명령어를 통해 확인합니다.
    ```
    kubectl get pod -n kube-system | egrep -o "alb-ingress[a-zA-Z0-9-]+"

    # 결과값 예시
    alb-ingress-controller-xxxxxxxxx
    ```
    클러스터 내부에서 필요한 기능들을 위해 실행되는 파드들을 애드온(Addon)이라고 합니다. 애드온에 사용되는 파드들은 디플로이먼트, 리플리케이션 컨트롤러 등에 의해 관리됩니다. 그리고 이 애드온이 사용하는 네임스페이스가 kube-system입니다.

    alb-ingress-controller 매니페스트에서 네임스페이스를 kube-system으로 명시했기에 위의 명령어로 파드 이름이 도출되면 정상적으로 배포된 것입니다. 또한, 아래의 명령어로 관련 로그를 확인할 수 있습니다. 
    ```
    kubectl logs -n kube-system $(kubectl get po -n kube-system | egrep -o "alb-ingress[a-zA-Z0-9-]+")
    ```
    또한, 아래의 명령어로 자세한 속성 값을 파악할 수 있습니다.
    ```
    kubectl describe pod -n kube-system {alb-ingress-controller pod name}

    # 입력값 예시
    kubectl describe pod -n kube-system alb-ingress-controller-xxxxxxxxx
    ```