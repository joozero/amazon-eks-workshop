---
date: 2020-09-03
title: "Cluster Autoscaler 적용하기"
weight: 720
pre: "<b>8-2  </b>"
---

#### Cluster Autoscaler를 사용하여 클러스터 스케일링 적용하기 
이전 페이지에서 파드에 오토 스케일링을 적용하였습니다. 하지만 트래픽에 따라 파드가 올라가는 클러스터의 자원이 모자라게 되는 경우도 발생하게 됩니다. 즉, 워커 노드가 가득 차서 파드가 스케줄될 수 없는 상태가 되는 것이죠. 이때, 사용하는 것이 **Cluster Autoscaler(CA)** 입니다.

# 그림 넣기

Cluster Autoscaler의 경우, unscheduled 파드가 존재할 경우, 워커 노드를 스케일 아웃하고, 특정 시간을 간격으로 사용률을 확인하여 스케일 인/아웃을 수행합니다. 그리고 AWS에서는 Auto Scaling Group을 사용하여 CA를 적용합니다.

* * *
