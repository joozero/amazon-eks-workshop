---
date: 2020-09-03
title: "두번째 백앤드 배포하기"
weight: 620
pre: "<b>7-2.  </b>"
---

#### express 백앤드 배포하기

flask 백앤드와 마찬가지로 express 백앤드도 배포합니다. 순서는 아래와 같습니다.
1. 아래의 명령어를 통해, 컨테이너라이징할 소스 코드를 다운 받습니다.
    ```
    git clone https://github.com/joozero/amazon-eks-nodejs.git
    ```
2. AWS CLI를 통해, 이미지 리포지토리를 생성합니다. 본 실습에서는 리포지토리 이름을 demo-nodejs-backend라고 설정합니다. 또한, 리전 값에는 EKS 클러스터를 배포할 리전 코드(예: ap-northeast-2)를 명시합니다.
    ```
    aws ecr create-repository \
    --repository-name demo-nodejs-backend \
    --image-scanning-configuration scanOnPush=true \
    --region ap-northeast-2
    ```
    그 다음 Amazon ECR에 이미지 올리는 방법은 [4-2 Amazon ECR에 이미지 올리기](https://master.d3s71i2n51x60t.amplifyapp.com/ko/container_image/push_to_ecr/) 가이드를 참고합니다. 이미지가 저장소에 적재되었다면 Amazon ECR 콘솔창에서 이미지를 확인할 수 있습니다. 
3. manifests 폴더 위치(/home/ec2-user/environment/manifests/)로 이동하여 **demo-nodejs-backend** 폴더를 만듭니다.
4. **demo-nodejs-backend** 폴더에서 아래의 값을 붙여넣습니다. 이 때, 이미지 값에는 **demo-nodejs-backend** 리포지토리 URI 값을 넣습니다. 해당 URI는 Amazon ECR 콘솔창에서 확인할 수 있습니다. 
    ```
    cat <<EOF> demo-nodejs-manifest.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: demo-nodejs-backend
    namespace: default
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: demo-nodejs-backend
    template:
        metadata:
        labels:
            app: demo-nodejs-backend
        spec:
        containers:
            - name: demo-nodejs-backend
            image: "415718916760.dkr.ecr.ap-northeast-2.amazonaws.com/demo-nodejs-backend:latest"
            imagePullPolicy: Always
            ports:
                - containerPort: 3000

    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: demo-nodejs-backend
    annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/"
    spec:
    selector:
        app: demo-nodejs-backend
    type: NodePort
    ports:
        - port: 8080
        targetPort: 3000
        protocol: TCP

    ---
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: "nodejs-backend-ingress"
    namespace: default
    annotations:
        kubernetes.io/ingress.class: alb
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
    labels:
        app: demo-nodejs-backend
    spec:
    rules:
        - http:
            paths:
            - path: /services/*
                backend:
                serviceName: "demo-nodejs-backend"
                servicePort: 8080
    EOF
    ```
5. 매니페스트를 배포합니다.
    ```
    kubectl apply -f demo-nodejs-manifest.yaml
    ```