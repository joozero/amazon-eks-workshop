---
date: 2020-08-11
title: "실습 환경 구축"
chapter: false
weight: 210
pre: "<b>4-1 </b>"
---

## 실습 환경 구축하기
{{% notice info %}}
**SAM(Serverless Application Model)**과 **Step function**을 이용하여 손쉽게 MLOPS 파이프라인을 구축 할 수 있습니다.

본 페이지에서는 Cloud9 을 이용해 SAM 코드를 수정하고 배포 할 환경을 구축 합니다.

{{% /notice %}}

### Cloud9 환경 생성
AWS 콘솔에서 **Cloud9**을 검색하여 선택 후 **Create environment** 버튼을 누릅니다.

![Cloud9 new environment](/images/preparation/c9newenv.png)

**Name**칸에 원하는 이름을 넣고 **Next step** 버튼을 누릅니다.

![Cloud9 new environment](/images/preparation/c9newenv2.png)

설정은 default로 수정하지 않고 **Next step** 버튼을 누릅니다.

![Cloud9 new environment](/images/preparation/c9newenv3.png)

원하는 환경 설정이 맞는지 확인 하고 **Create environemnt** 버튼을 눌러 Cloud9 환경을 생성 합니다.

### SAM CLI 설치
SAM CLI 설치를 위해서 먼저 Homebrew를 설치 합니다. 다음의 코드를 Cloud9 IDE의 terminal에서 실행 합니다.
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```
PATH에 Homebrew를 추가하기 위해 다음의 코드를 실행 합니다.
```
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
```
Homebrew가 잘 설치 되었는지 확인합니다.
```
brew --version
```
정상적으로 설치 되었다면 아래와 비슷한 메세지를 확인 하실 수 있습니다.
``` shell
Homebrew 2.4.11
Homebrew/linuxbrew-core (git revision 037fd; last commit 2020-08-12)
```
Homebrew가 정상적으로 설치 되었다면 다음 코드를 terminal에서 실행하여 **AWS SAM CLI** 를 설치 합니다.
```
brew tap aws/tap
brew install aws-sam-cli
```
Terminal에서 다음 명령어를 실행하여 SAM CLI가 정상적으로 설치 되었는지 확인 합니다.
```
sam --version
```
SAM CLI가 정상적으로 설치 되었다면 다음과 같은 메세지를 확인 할 수 있습니다.
``` bash
SAM CLI, version 1.0.0
```