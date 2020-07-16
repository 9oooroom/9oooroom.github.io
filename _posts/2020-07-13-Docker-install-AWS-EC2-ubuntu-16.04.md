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
1.1 OS requirements
* Ubuntu Xenial 16.04 (LTS) 사용

1.2 Uninstall old versions
기존에 있던 구버전 docker 삭제 확인
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

1.3 install
* [Docker의 리포지토리](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)를 설정하고 설치
* DEB 패키지를 다운로드 하여 수동 설치
해당 문서에서는 Docker 리포지토리를 설정하여 설치를 진행한다.

1.4 리포지토리 설정
1.4.1HTTPS를 통해 저장소를 사용할수 있도록 apt 패키지 색인을 업데이트 한다.

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
