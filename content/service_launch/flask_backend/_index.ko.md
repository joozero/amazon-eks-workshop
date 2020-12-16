---
date: 2020-09-03
title: "첫번째 백앤드 배포하기"
weight: 610
pre: "<b>7-1  </b>"
---

#### flask 백앤드 배포하기

{{% notice info %}}
본 실습을 진행하기 위해 **4-2 Amazon ECR에 이미지 올리기** 실습 부분이 선행되어야 합니다.
{{% /notice %}}

1. **manifests 폴더** 위치(/home/ec2-user/environment/manifests)로 이동합니다.
    ```
    cd ..
    ```

2. 다음 명령어를 수행하여 manifest 를 생성합니다.
    ```
    cat <<EOF> flask-deployment.yaml
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
              image: {demo-flask-backend repository URI:latest}
              imagePullPolicy: Always
              ports:
                - containerPort: 8080
    EOF
    ```
3. 해당 파일을 열어 **이미지** 값에 [4-2 Amazon ECR에 이미지 올리기](../../container_image/push_to_ecr/)에서 생성한 **리포지토리 URI** 값을 넣습니다. 해당 URI는 [Amazon ECR 콘솔창](https://ap-northeast-2.console.aws.amazon.com/ecr/repositories/demo-flask-backend/?region=ap-northeast-2)에서 확인할 수 있습니다.

    ![](/images/service_launch/ecr-uri.png)


4. 그 다음 service 매니페스트 파일을 생성하기 위해 아래의 값을 붙여 넣습니다.
    ```
    cat <<EOF> flask-service.yaml
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

5. 마지막으로 ingress 매니페스트 파일을 생성하기 위해 아래의 값을 붙여 넣습니다.
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
    EOF
    ```

6.  위에서 생성한 매니페스트를 아래의 순서대로 배포합니다.
    ```
    kubectl apply -f flask-deployment.yaml
    kubectl apply -f flask-service.yaml
    kubectl apply -f ingress.yaml
    ```
3. 다음 명령어 수행 결과를 웹 브라우저 및 API 플랫폼에 붙여넣어 확인합니다. 
    ```
    echo http://$(kubectl get ingress/backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/contents/aws
    ```
{{% notice info %}}
인그레스 오브젝트가 배포되는 동안 약간의 시간이 소요됩니다. [EC2 콘솔창](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LoadBalancers:)에서 **Load Balancers** 상태가 active가 될 때까지 기다립니다.
{{% /notice %}}