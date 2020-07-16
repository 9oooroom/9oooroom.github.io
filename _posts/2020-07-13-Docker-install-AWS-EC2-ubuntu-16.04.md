---
layout: default
title: "Docker install AWS EC2 Ubuntu 16.04"
date: 2020-07-16 19:30:00 +0200
published: 2020-07-13 19:30:00 +0200
comments: true
categories: Docker
tags: [Docker, AWS, Ubuntu]
---


도커를 이용한 개발에서는 호스트 운영 체제 위에 여러 도커 컨테이너를 전개하고 이 컨테이너끼리 통신을
주고받는 형식으로 애플리케이션을 구축하는 경우가 많다. 이런 방식에는 개발용 머신 리소스를 효율적으로 활용
한다는 면에서도 하이퍼바이저형 가상화 기술이 적합하다. 개발 과정에서 도커 컨테이너를 빈번하게 생성하고
파기할 수 있다는 것도 중요한 점이다. 

EC2 ubuntu instance에 Docker를 설치해보자.

<!--more-->

#### 1. Prerequisites
**1.1 OS requirements**
* Ubuntu Xenial 16.04 (LTS) 사용
<br/>

**1.2 Uninstall old versions**

기존에 있던 구버전 docker 삭제 확인
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```
<br/>

**1.3 install**
* [Docker의 리포지토리](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)를 설정하고 설치
* DEB 패키지를 다운로드 하여 수동 설치
해당 문서에서는 Docker 리포지토리를 설정하여 설치를 진행한다.

<br/>

**1.4 리포지토리 설정**

**1.4.1 HTTP를 이용한 설치**

```
$ sudo apt-get update
```
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
<br/>

**1.4.2 Docker의 공식 GPG Key 값 추가**
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
지문 중 **'0EBF CD88'** 해당 지문 키가 있는지 확인 한다.

```
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```
<br/>

**1.4.3 레포지토리 추가하기**
```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
<br/>

**1.4.4 Docker ENGINE Install**

apt 패키지 색인을 업데이트 하고 최신 버전의 Docker Engine 및 컨테이너를 설치
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
<br/>

**특정 버전 install**

```
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

<br/>


**1.4.5 설치 확인**

'hello-world' 이미지를 실행하여 docker engine이 설치되었는지 확인 한다.
```
$ sudo docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
<br/>

출처 : [Docker Docs - Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)