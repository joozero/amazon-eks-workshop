---
date: 2020-09-03
title: "Kubernetes(k8s) 란?"
weight: 11
pre: "<b>  </b>"
---

![k8s logo](/images/overview/k8s_logo.png)

### Kubernetes에 대하여 

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼입니다. 쿠버네티스는 선언적 구성과 자동화를 모두 용이하게 해주는 컨테이너 오케스트레이션 툴입니다.

쿠버네티스에 대해 더 자세히 알고 싶다면 [여기](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) 를 클릭하세요.

* * *

### Kubernetes Cluster

![k8s compoent](/images/overview/k8s_component.png)

쿠버네티스를 배포하면 **클러스터**를 얻습니다. 그리고 이 클러스터는 **노드들의 집합**입니다. 노드들은 크게 두 가지 유형으로 나눠지는데, 각각이 **컨트롤 플레인**과 **데이터 플레인**입니다.

- 컨트롤 플레인(Control Plane)은 워커 노드와 클러스터 내 파드를 관리하고 제어합니다.
- 데이터 플레인(Data Plane)은 **워커 노드들**로 구성되어 있으며 컨테이너화된 애플리케이션의 구성 요소인 **파드**를 호스트합니다.

* * *

### Kubernetes Objects

쿠버네티스의 오브젝트는 **바라는 상태(desired state)를 담은 레코드**입니다. 오브젝트를 생성하면 쿠버네티스의 컨트롤 플레인에서 오브젝트의 **현재 상태(current state)** 와 바라는 상태를 일치시키기 위해 끊임없이 관리합니다.

쿠버네티스의 객체에는 파드(pod), 서비스(service), 디플로이먼트(Deployment) 등이 있습니다.

{{% notice note %}}
보다 자세한 내용은 [쿠버네티스 공식 홈페이지](https://kubernetes.io/)를 통해 확인하세요.
{{% /notice %}}



