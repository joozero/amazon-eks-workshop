---
date: 2020-09-03
title: "백앤드 배포하기"
weight: 610
pre: "<b>7-1  </b>"
---

#### flask 백앤드 배포하기

1. **manifests** 폴더 위치(/home/ec2-user/environment/manifests/)로 이동하여 **demo-flask-backend** 폴더를 만듭니다.
    ```
    mkdir demo-flask-backend
    ```
2. **demo-flask-bakckend** 폴더에서 아래의 값을 붙여넣습니다. 이 때, **이미지** 값에는 [4-2 Amazon ECR에 이미지 올리기](https://master.d3s71i2n51x60t.amplifyapp.com/ko/container_image/push_to_ecr/)에서 생성한 리포지토리 URI 값을 넣습니다. 해당 URI는 Amazon ECR 콘솔창에서 확인할 수 있습니다.
    ```
    cat <<EOF> deployment.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: demo-flask-backend
    namespace: default
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: demo-flask-backend
    template:
        metadata:
        labels:
            app: demo-flask-backend
        spec:
        containers:
            - name: demo-flask-backend
            image: "{aws_account_id}.dkr.ecr.ap-northeast-2.amazonaws.com/demo-flask-backend:latest"
            imagePullPolicy: Always
            ports:
                - containerPort: 8080
    EOF
    ```
3. 그 다음 service 매니페스트 파일을 생성하기 위해 아래의 값을 붙여 넣습니다.
    ```
    cat <<EOF> service.yaml
    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: demo-flask-backend
    annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/"
    spec:
    selector:
        app: demo-flask-backend
    type: NodePort
    ports:
        - port: 8080
        targetPort: 8080
        protocol: TCP
    EOF
    ```
4. 마지막으로 ingress 매니페스트 파일을 생성하기 위해 아래의 값을 붙여 넣습니다.
    ```
    cat <<EOF> ingress.yaml
    ---
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
    name: "flask-backend-ingress"
    namespace: default
    annotations:
        kubernetes.io/ingress.class: alb
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
    labels:
        app: demo-flask-backend
    spec:
    rules:
        - http:
            paths:
            - path: /contents/*
                backend:
                serviceName: "demo-flask-backend"
                servicePort: 8080
    EOF
    ```
5. 위에서 생성한 매니페스트를 아래의 순서대로 배포합니다.
    ```
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    kubectl apply -f ingress.yaml
    ```

* * *

#### 현재의 아키텍처
flask 백앤드를 배포한 후, 현재까지 구성된 서비스의 아키텍처는 아래와 같습니다.

![current architecture](/images/service_launch/current-architecture.svg)
