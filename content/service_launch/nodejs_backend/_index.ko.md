---
date: 2020-09-03
title: "두번째 백앤드 배포하기"
weight: 620
pre: "<b>7-2  </b>"
---

#### express 백앤드 배포하기

flask 백앤드와 마찬가지로 express 백앤드도 배포합니다. 순서는 아래와 같습니다.

1. 아래의 명령어를 통해, 컨테이너라이징할 소스 코드를 다운 받습니다.
    ```
    cd /home/ec2-user/environment
    git clone https://github.com/joozero/amazon-eks-nodejs.git
    ```

2. AWS CLI를 통해, 이미지 리포지토리를 생성합니다. 본 실습에서는 리포지토리 이름을 **demo-nodejs-backend**라고 설정합니다. 또한, 리전 값에는 EKS 클러스터를 배포할 리전 코드(예: ap-northeast-2)를 명시합니다.
    ```
    aws ecr create-repository \
    --repository-name demo-nodejs-backend \
    --image-scanning-configuration scanOnPush=true \
    --region ap-northeast-2
    ```

3. 그 다음 Amazon ECR 리포지토리에 컨테이너 이미지를 푸시합니다. 자세한 설명은 [4-2 Amazon ECR에 이미지 올리기](../../container_image/push_to_ecr/) 가이드를 참고합니다. 

    ```
    cd amazon-eks-nodejs
    docker build -t demo-nodejs-backend .
    docker tag demo-nodejs-backend:latest ${ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/demo-nodejs-backend:latest
    docker push ${ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/demo-nodejs-backend:latest
    ```

* * *

1. **manifests 폴더** (/home/ec2-user/environment/manifests)로 이동하여 아래의 값을 붙여넣습니다. 그 뒤 `nodejs-deployment.yaml` 파일을 열어 `image` 부분의 ACCOUNT_ID를 본인 것으로 교체합니다.
    ```
    cd /home/ec2-user/environment/manifests

    cat <<EOF> nodejs-deployment.yaml
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
              image: $ACCOUNT_ID.dkr.ecr.ap-northeast-2.amazonaws.com/demo-nodejs-backend:latest
              imagePullPolicy: Always
              ports:
                - containerPort: 3000
    EOF
    ```

    
    ```
    cat <<EOF> nodejs-service.yaml
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
    EOF
    ```
    [!] 인그레스 매니페스트 파일은 [7-1 첫 번째 백앤드 배포하기](../../service_launch/flask_backend/)에 있는 파일을 수정합니다. 

    ```
    cat <<EOF> ingress.yaml
    ---
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
      name: "backend-ingress"
      namespace: default
      annotations:
        kubernetes.io/ingress.class: alb
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
    spec:
      rules:
        - http:
            paths:
              - path: /contents/*
                backend:
                  serviceName: "demo-flask-backend"
                  servicePort: 8080
              - path: /services/*
                backend:
                  serviceName: "demo-nodejs-backend"
                  servicePort: 8080
    EOF
    ```

2. 매니페스트를 배포합니다.
    ```
    kubectl apply -f nodejs-deployment.yaml
    kubectl apply -f nodejs-service.yaml
    kubectl apply -f ingress.yaml
    ```

3. 다음 명령어 수행 결과를 웹 브라우저 및 API 플랫폼에 붙여넣어 확인합니다. 
    ```
    echo http://$(kubectl get ingress/backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/services/all
    ```