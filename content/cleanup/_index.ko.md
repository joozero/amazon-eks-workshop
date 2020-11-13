---
title: "실습 리소스 정리"
weight: 1000
pre: "<b>   </b>"
---

{{% notice warning %}}
이 실습을 마치면 사용한 AWS 계정에 비용이 추가로 발생하지 않도록 사용한 리소스를 삭제해야 합니다.
{{% /notice %}}

1. 인그레스 자원들을 삭제합니다.
    ```
    kubectl delete -f 
    ```
2. 생성한 eks 클러스터를 삭제합니다.
    ```
    eksctl delete cluster --name=eks-demo
    ```
3. 생성한 ECR 저장소를 삭제합니다.
    아래의 명령어를 통해, 생성한 저장소 리스트를 불러옵니다.
   ```
    aws ecr describe-repositories
    ```
4. 생성한 Cloud9 IDE 환경을 삭제합니다.