---
layout: default
title: "Deploy AWS Docker Containers"
date: 2020-07-19 16:00:00 +0200
published: 2020-07-19 16:00:00 +0200
comments: true
categories: Docker
tags: [Docker, AWS, Ubuntu]
---

앞서 Docker Engine을 install 하였다. 이번에는 Docker의 기본 조작 방법을 익히고
애플리케이션을 배포하는 과정을 진행해 볼 예정이다.

<!--more-->

1.컨테이너로 애플리케이션 실행하기

<style type="text/css">
.tg  {border-collapse:collapse;border-color:#ccc;border-spacing:0;}
.tg td{background-color:#fff;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f0f0f0;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-dgfm{background-color:#c0c0c0;border-color:inherit;font-size:12px;text-align:left;vertical-align:top}
.tg .tg-73a0{border-color:inherit;font-size:12px;text-align:left;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-dgfm">개념</th>
    <th class="tg-dgfm">역할</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-73a0">도커 이미지<br></td>
    <td class="tg-73a0">도커 컨테이너를 구성하는 파일 시스템과 실행할 애플리케이션 설정을<br>하나로 합친 것으로, 컨테이너를 생성하는 템플릿 역할을 한다.</td>
  </tr>
  <tr>
    <td class="tg-73a0">도커 컨테이너</td>
    <td class="tg-73a0">도커 이미지를 기반으로 생성되며, 파일 시스템과 애플리케이션이 구체화돼<br>실행되는 형태</td>
  </tr>
</tbody>
</table>

예를 들어 Ubuntu 파일 시스템을 이용한 정의한 도커 이미지로 도커 컨테이너를 생성하면 아래 그림과 같다.

<a href="/assets/images/{{page.id}}/container-and-image.jpg"> <img
	class="center-block img-responsive"
	src="/assets/images/{{page.id}}/container-and-image.jpg" alt="container-image"/>
</a>

도커 이미지 하나로 여러 개의 컨테이너를 생성할 수 있다.
위 그림에서 도커 이미지는 우분투 파일 시스템을 실행하는 애플리케이션과 파일을 담고 있다.
컨테이너가 생성될 때 이미지로 부터 구체화하고 컨테이너 안의 우분투 파일 시스템상에서 애플리케이션이 실행 된다.
컨테이너로 애플리케이션을 실행하려면 컨테이너 형태로 구체화될 템플릿 역할을 하는 이미지를 먼저 만들어야 한다.

2.도커 컨테이너 만들기

