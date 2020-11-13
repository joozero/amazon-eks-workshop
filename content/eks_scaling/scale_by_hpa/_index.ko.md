---
date: 2020-09-03
title: "HPA 적용하기"
weight: 710
pre: "<b>8-1  </b>"
---

#### HPA를 사용하여 파드 스케일링 적용하기
HPA(Horizontal Pod Autoscaler) 컨트롤러는 메트릭 값에 값에 따라 파드의 개수를 할당합니다. 파드 스케일링을 적용하기 위해 컨테이너에 필요한 리소스 양을 명시하고, HPA를 통해 스케일할 조건을 작성해야 합니다.
![hpa scaling](/images/eks_scaling/k8s-scaling.svg)

* * *

1. 쿠버네티스 metrics server를 생성합니다.
    ```
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
    ```
2. metrics server가 정상적으로 생성되었는지 확인합니다.
    ```
    kubectl get deployment metrics-server -n kube-system
    ```
3. 그 다음, [7-1 첫번째 백앤드 배포하기](../../service_launch/flask_backend/)에서 만들었던 flask deployment yaml 파일을 아래와 같이 수정합니다.
    ```
    cat <<EOF> flask-deployment.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: demo-flask-backend
      namespace: default
    spec:
      replicas: 1
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
              resources:
                requests:
                  cpu: 250m
                limits:
                  cpu: 500m
    EOF
    ```
    위의 작업을 통해, 레플리카를 1로 설정하고 컨테이너에 필요한 리소스 양을 설정합니다.
{{% notice info %}}
1vCPU = 1000m(milicore)
{{% /notice %}}

4. yaml 파일을 적용하여 변경 사항을 반영합니다.
    ```
    kubectl apply -f flask-deployment.yaml
    ```
5. HPA를 설정하기 위해, 아래의 yaml 파일도 생성합니다.
    ```
    cat <<EOF> flask-hpa.yaml
    ---
    apiVersion: autoscaling/v1
    kind: HorizontalPodAutoscaler
    metadata:
      name: demo-flask-backend-hpa
      namespace: default
    spec:
      scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: demo-flask-backend
      minReplicas: 1
      maxReplicas: 5
      targetCPUUtilizationPercentage: 30
    EOF
    ```
    해당 yaml 파일을 반영합니다. 
    ```
    kubectl apply -f flask-hpa.yaml
    ```

    아래와 같이 kubectl로 간단하게 설정할 수도 있습니다.
    ```
    kubectl autoscale deployment demo-flask-backend --cpu-percent=30 --min=1 --max=5
    ```
6. HPA를 생성한 다음, 아래의 명령어로 HPA 상태를 확인할 수 있습니다. target에서 CPU 사용률이 unknown으로 나올 경우, 잠시 기다린 후, 확인합니다.
    ```
    kubectl get hpa
    ```
7. 오토스케일링 기능이 정상적으로 작동하는지 확인하기 위해 간단한 부하 테스트를 진행합니다. 이 때, Cloud9에서 새로운 터미널을 추가로 생성하여 기존의 터미널 창에 아래의 명령어를 입력합니다.

    아래의 backend-ingress 주소는 **kubectl get ingress/backend-ingress** 명령어를 통해 출력된 주소 값을 입력합니다.
    ```
    while true; do curl http://{backend-ingress ADDRESS value}/contents/aws; done
    ```
    방금 생성한 터미널 창에 아래의 명령어를 입력합니다.
    ```
    kubectl get hpa -w
    ```
    ![hpa scaling load test](/images/eks_scaling/hpa-scaling-cloud9.png)
8. 부하 테스트를 진행하는 동안, CPU 사용률에 따라 파드의 개수가 변화하는 것을 확인할 수 있습니다. 부하 테스트를 완료하면 ctrl+c를 통해 작동을 중지시킵니다.