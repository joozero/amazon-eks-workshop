---
date: 2020-09-03
title: "Amazon EKS"
weight: 12
pre: "<b>  </b>"
---

![eks logo](/images/overview/eks_logo.png)

### Amazon EKS에 대하여 

[Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)는 Kubernetes를 쉽게 실행할 수 있는 관리형 서비스입니다. AWS 환경에서 Kubernetes 컨트롤 플레인 또는 노드를 직접 설치, 운영 및 유지할 필요가 없습니다.

![eks architecture](/images/overview/eks_architecture.svg)

Amazon EKS는 여러 가용 영역에서 Kubernetes 컨트롤 플레인 인스턴스를 실행하여 고가용성을 보장합니다. 또한, 비정상 컨트롤 플레인 인스턴스를 자동으로 감지하고 교체하며 자동화된 버전 업그레이드 및 패치를 제공합니다.

Amazon EKS는 다양한 AWS 서비스들과 연동하여 애플리케이션에 대한 확장성 및 보안을 제공하는 서비스를 제공합니다.

- 컨테이너 이미지 저장소인 **Amazon ECR(Elastic Container Registry)**
- 로드 분산을 위한 **AWS ELB(Elastic Load Balancing)**
- 인증을 위한 **AWS IAM**
- 격리된 **Amazon VPC**


Amazon EKS는 오픈 소스 Kubernetes 소프트웨어의 최신 버전을 실행하므로 Kubernetes 커뮤니티에서 사용되는 플러그인과 툴을 모두 사용할 수 있습니다. 온프레미스 데이터 센터에서 실행 중인지 퍼블릭 클라우드에서 실행 중인지에 상관없이, Amazon EKS에서 실행 중인 애플리케이션은 표준 Kubernetes 환경에서 실행 중인 애플리케이션과 완벽하게 호환됩니다. 즉, 코드를 수정하지 않고 표준 Kubernetes 애플리케이션을 Amazon EKS로 쉽게 마이그레이션할 수 있습니다.