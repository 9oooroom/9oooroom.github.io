---
layout: default
title: "Terraform Summary"
date: 2020-11-19 20:21:00 +0200
published: 2020-11-19 20:21:00 +0200
comments: true
categories: Terraform
tags: [Terraform, Cloud]
---
본인의 경우에는 AWS로 Cloud를 접했고 현재는 NCP을 이용하여 고객들에게 서비스를 구현하는 아키텍처 일을 맡고 있다. 거기서 오는 밴더 간 차이들을 매번 신경써야 하는 부분이 있어 Terraform을 사용하려고 한다.

<!--more-->

1. Terraform<br/>

인프라를 안전하고 효율적으로 구축,변경,버전관리를 하기 위한 프로비저닝 도구이다.
구성파일은 단일 애플리케이션 또는 전체 데이터 센터를 실행하는데 필요한 구성요소들을 Terrafrom에 정의 한다.
원하는 인프라 또는 애플리케이션의 형태를 구축하기 위해 HCL로 정의 하여 구축 및 업데이트를 진행 할 수 있다.
<br/>
<br/>

2. Infrastructure as Code<br/>
인프라는 고급 구성 구문을 사용하여 설명된다. 
이를 통해 데이터 센터의 블루프린트를 다른 코드처럼 버전화하고 처리 할 수 ​​있다. 
또한 인프라를 공유하고 재사용 할 수 있다.
<br/>
<br/>

3. Execution Plans<br/>

기본적으로 Terraform은 plan 기능을 가지고 있다.
HCL로 인프라 작성을 마치고 terraform plan 명령어를 이용하여 미리 만들어질 인프라의 상태를 확인 할 수 있다.
<br/>
<br/>

4. Resource Graph<br/>

Terraform은 모든 리소스의 그래프를 작성하고 모든 비 종속 리소스의 생성 및 수정을 병렬화한다.
때문에 Terraform은 가능한 한 효율적으로 인프라를 구축하고 운영자는 인프라의 종속성을 알 수 있다.
<br/>
<br/>

5. Change Automation<br/>

앞의 내용을 토대로 실행 계획 그리고 리소스 그래프를 사용하면 Terraform이 어떤 순서로 인프라 변경을 하는지 정확하게 파악이 가능하다.
이로 인해 사람이 직접 구축하는 인프라에 비해 오류가 적다.
<br/>
<br/>


다음은 Terraform을 사용하기 위해 Windows에 Terraform을 설치하는 법을 알아보겠다.
