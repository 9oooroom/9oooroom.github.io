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

##### 순서
1.AWS EC2 Ubuntu 16.04에 Docker install

\~\~~
curl -fsSL https://get.docker.com/ | sudo sh
~~\~\