---
date: 2020-09-03
title: "그 외의 다른 툴들 설치하기"
weight: 260
pre: "<b>  </b>"
---

#### jq 설치하기
jq는 JSON 형식의 데이터를 다루는 커맨드라인 유틸리티입니다. 아래의 명령어를 통해, jq를 설치합니다.
```
sudo yum install -y jq
```

#### bash-completion 설치하기
Bash 쉘에서 kubectl completion script는 **kubectl completion bash** 명령어를 통해 생성할 수 있습니다. 쉘에 completion script를 소싱하면 kubectl 명령어의 자동 완성을 가능하게 만들 수 있습니다. 하지만 이런 completion script는 bash-completion에 의존하기 때문에 아래의 명령어를 통해, [bash-completion](https://github.com/scop/bash-completion#installation)을 설치해야 합니다.
```
sudo yum install -y bash-completion
```