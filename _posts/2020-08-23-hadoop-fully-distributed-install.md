---
layout: default
title: "Hadoop Fully Distributed install"
date: 2020-08-23 12:26:00 +0200
published: 2020-08-23 12:26:00 +0200
comments: true
categories: Hadoop
tags: [Hadoop, BigData]
---

빅데이터분석이 쉬워진것은 하둡(Hadoop)이 개발되면서 부터이다.
하둡은 대용량 데이터를 적은비용으로 더 빠르게 분석할 수 있는 소프트웨어이며, 빅데이터 처리와 분석을 위한 플랫폼 중 사실상 표준으로 자리잡고 있다
여러대의 컴퓨터로 데이터를 분석하고 저장하는 방식으로 분석에 필요했던 많은 비용과 시간을 단축할 수 있다.
페이스북의 자동 이미지검색,금융거래 내역 분석을 통한 사기 방지, 검색 패턴을 통한 광고타겟 및 마케팅 등 여러 분야에서 활용 될 수 있다.

<!--more-->

## Prerequisties

* OS : CentOS7
* Config
    * Master Name Node : 1EA
    * Secondary Name Node : 1EA
    * Slave Data Node : 2EA
* Version
    * Java : 1.8.0
    * Hadoop : 2.7.7


## Progress

1. CentOS 7 install basic setting
1.1 hostname
1.2 firewalld
1.3 timezone
2. java 1.8.0 install
3. PATH add
4. ssh config
5. hadoop 2.7.7 install
5.1. hadoop-env.sh
5.2. yarn-env.sh
5.3. core-site.xml
5.4. hdfs-site.xml
5.5. mapred-site.xml
5.6. yarn-site.xml
5.7. slave
6. slave 배포
6.1. hadoop config files / profile
7.실행 및 테스트


1. CentOS 7 install basic setting
- 생략

2. Java 1.8.0 install
* 자바 설치


    '''
    $ java -version
    $ rpm -qa | grep jdk
    java version "1.8.0_261"
    Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
    Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
    '''


3. PATH add
* 환경변수 저장


    '''
    $ vi /etc/profile
    #하둡 home
    export HADOOP_HOME=/usr/local/hadoop-2.7.7
    #jdk home
    export JAVA_HOME=/usr/local/jdk1.8.0
    export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
    export CLASSPATH=$JAVA_HOME/lib:$CLASSPATH
    '''

4. ssh config
* ssh-keygen으로 공개키/비밀키를 생성하고, 공개키 내용을 접속할 서버에 저장하면 해당 서버에 Password 없이 SSH 접속이 가능하다.
    4.1 모든 서버에 ssh 키젠 설치

    ```
    $ ssh-keygen -t rsa
    ```

    ~/.ssh/id_rsa.pub 에 공개키가 저장 된다, 이 공개키의 내용을 접속할 서버의 ~/.ssh/authorized_keys 에 추가 할 것이다.
    접속할 서버의 authorized_keys 파일에 공개키 추가.
    ```
    $ ssh-copy-id [계정]@[접속할 서버 IP]
    ```
    ex) ssh-copy-id root@hadoop-slave1

    4.2 ssh를 통해 접속 확인
    ```
    $ ssh [계정]@[접속할 서버 IP]
    ```
    암호를 묻지 않는다면 올바르게 설정이 된 것임. 각각 모든 서버에 서로 전부 되도록 설정 (자기자신의 서버에도 적용)

5. Hadoop Install
    * Namenode 구성 후 datanode로 배포 예정
    * 설정파일 경로 : /usr/local/hadoop-2.7.7/etc/hadoop

    5.1 hadoop-env.sh

        ```
        export HADOOP_HOME=/usr/local/hadoop-2.7.7
        export HADOOP_MAPRED_HOME=$HADOOP_HOME
        export HADOOP_COMMON_HOME=$HADOOP_HOME
        export HADOOP_HDFS_HOME=$HADOOP_HOME
        export YARN_HOME=$HADOOP_HOME
        export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
        export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
        export JAVA_HOME=/usr/local/jdk1.8.0
        ```

    5.2 yarn-env.sh

        ```
        export JAVA_HOME=/usr/local/jdk1.8.0
        export HADOOP_HOME=/usr/local/hadoop-2.7.7
        export HADOOP_MAPRED_HOME=$HADOOP_HOME
        export HADOOP_COMMON_HOME=$HADOOP_HOME
        export HADOOP_HDFS_HOME=$HADOOP_HOME
        export YARN_HOME=$HADOOP_HOME
        export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
        export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
        ```

    5.3 core-site.xml

        ```
        <configuration>
            <property>
                <name>fs.default.name</name>
                <value>hdfs://hadoop-master1:9000</value>
            </property>
            <property>
                <name>hadoop.tmp.dir</name>
                <value>/usr/local/hadoop-2.7.7/tmp</value>
            </property>
        </configuration>
        ```

        + 추가 : Cloud Object Storage 연동을 위하여 해당 core-site.xml 파일의 수정이 필요로 하다. 해당파일의 hdfs 경로를
            s3 경로 / AccessKey / SecretAccessKey 항목으로 변환이 필요하다.
            아래 코드는 그 예시를 보여준다.

        ```
        <congiguration>
            <property> 
                <name>fs.s3.impl</name> #해당 스키마도 하둡 버전에 따라 상이함
                <value>com.amazon.ws.emr.hadoop.fs.S3FileSystem</value> 
            </property> 
            <property> 
                <name>fs.s3.awsAccessKeyId</name> 
                <value>액세스키</value> 
            </property> 
            <property> 
                <name>fs.s3.awsSecretAccessKey</name> 
                <value>시크릿키</value>
            </property>
        </configuration>
        ```

    5.4 hdfs-site.xml

        ```
        <configuration>
            <property>
                <name>dfs.replication</name>
                <value>3</value>
            </property>
            <property>
                <name>dfs.permissions.enabled</name>
                <value>false</value>
            </property>
            <property>
                <name>dfs.webhdfs.enabled</name>
                <value>true</value>
            </property>
            <property>
                <name>dfs.namenode.http.address</name>
                <value>hadoop-master1:50070</value>
            </property>
            <property>
                <name>dfs.secondary.http.address</name>
                <value>hadoop-master1:50090</value>
            </property>
        </configuration>
        ``` 

    5.5 mapred-site.xml
        * 맵리듀스 파일은 해당 경로에 템플릿이 존재한다. 해당 템플릿을 복사하여 사용하자

        ```
        <configuration>
            <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
            </property>
        </configuration>
        ```

    5.6 yarn-site.xml

        ```
        <configuration>
        <!-- Site specific YARN configuration properties -->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
            <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
        <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
            <value>hadoop-master1:8025</value>
        </property>
        <property>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>hadoop-master1:8030</value>
        </property>
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>hadoop-master1:8040</value>
        </property>
        </configuration>
        ```

    5.7 slave

        ```
        hadoop-slave1
        hadoop-slave2
        ```

6. slave 배포
    6.1 hadoop config files / profile

        ```
        $ scp -r hadoop-2.7.7 root@hadoop-slave1:/usr/local
        $ scp -r hadoop-2.7.7 root@hadoop-slave2:/usr/local
        ```

        ```
        $ scp /etc/profile root@hadoop-slave1:/etc/profile
        $ scp /etc/profile root@hadoop-slave2:/etc/profile
        ```

        * 각 Slave 노드들에 Java / Hadoop 설치 확인

        ```
        $ java -version
        $ hadoop version
        ```

7. Hadoop 실행 및 테스트
    7.1 실행

        ```
        $ hadoop namenode -format
        ```

        * ~$HADOOP_HOME/sbin/start-all.sh 으로 실행 ( 중지는 stop-all.sh )
        * 하둡 현황 파악 : localhost:8088
        * HDFS 현황 파악 : localhost:50070
        * jps 명령어를 통해 namenode / datanode 의 동작 프로세스를 확인 가능

    7.2 Wordcount 테스트

        ```
        $ hdfs dfs -mkdir /input
        $ hdfs dfs -copyFromLocal /usr/local/hadoop-2.7.7/READNE.txt /input
        ```

        하둡 파일시스템에 /input 디렉토리를 생성하고 /input에 로컬의 README.txt 업로드

        ```
        $ hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.2.jar wordcount /input/README.txt ~/wordcount-output
        ```

        맵리듀스 작업이 진행되고 결과를 cmd창에 출력한다.

        hdfs 현황웹에서 wordcount-output 결과를 확인 할 수 있다.
