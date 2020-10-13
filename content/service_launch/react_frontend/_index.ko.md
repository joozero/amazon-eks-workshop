---
date: 2020-09-03
title: "프론트앤드 배포하기"
weight: 630
pre: "<b>7-3.  </b>"
---

#### react 프론트앤드 배포하기

```
cat <<EOF> deployment.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-frontend
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo-frontend
  template:
    metadata:
      labels:
        app: demo-frontend
    spec:
      containers:
        - name: demo-frontend
          image: "415718916760.dkr.ecr.ap-northeast-2.amazonaws.com/demo-frontend:latest"
          imagePullPolicy: Always
          ports:
            - containerPort: 80
EOF
```

```
cat <<EOF> service.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: demo-frontend
  annotations:
    alb.ingress.kubernetes.io/healthcheck-path: "/"
spec:
  selector:
    app: demo-frontend
  type: NodePort
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 80
EOF
```

```
cat <<EOF> ingress.yaml
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: "frontend-ingress"
  namespace: default
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
  labels:
    app: demo-frontend
spec:
  rules:
    - http:
        paths:
          - path: /*
            backend:
              serviceName: "demo-frontend"
              servicePort: 80
EOF
```

