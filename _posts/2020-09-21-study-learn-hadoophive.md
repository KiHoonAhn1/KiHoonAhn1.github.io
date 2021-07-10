---
layout: post
title:  "[Hadoop&Hive] 하둡&하이브 설치&초기설정 방법"
subtitle:   "수업시간에 배운 Hadoop&Hive"
categories: study
tags: learn hadoop hive
comments: true
header-img: img/review/2019-07-08-review-book-game-algorithm-1.jpg
---

> 수업 시간에 배운 Hadoop & Hive 초기 설정 방법과 각 상황에서 일어난 문제 대처법

# HADOOP

## HADOOP 설치

빅데이터 처리 위해 태어난 분산시스템

![hadoop](md-images/hadoop.jpg)



### 그 전에 확인할 것 !

* ip 설정 확인

  * **vim /etc/sysconfig/network-scripts/ifcfg-ens33**

* jdk 설치 (Linux.md 파일에서 확인)

* hostname 변경

  * **hostnamectl set-hostname "name"**
  * **vim /etc/hosts** 에 IP와 name 넣기

* firewall 중지

  * **systemctl stop firewalld**

    **systemctl disable firewalld**



### 설치 시작

1. **wget https://archive.apache.org/dist/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz** 을 입력해 다운로드

2. **tar xvf hadoop-1.2.1.tar.gz**

3. **cp -r /usr/local/**

4. **vim /etc/profile** 을 변경한다

   ```bash
   JAVA_HOME=/usr/local/jdk1.8.0
   CLASSPATH=/usr/local/jdk1.8.0/lib
   HADOOP_HOME=/usr/local/hadoop-1.2.1
   export JAVA_HOME CLASSPATH HADOOP_HOME
   PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:.:$PATH
   ```

5. 보안 설정

   * **ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa**
   * **cat id_dsa.pub >> authorized_keys**

6. configuration 설정

   * **cd /usr/local/hadoop-1.2.1/conf**

   [**core-site.xml**]

   ```markdown
   <configuration>
   
   <property>
   <name>fs.default.name</name>
   <value>hdfs://localhost:9000</value>
   </property>
   
   <property>
   <name>hadoop.tmp.dir</name>
   <value>/usr/local/hadoop-1.2.1/tmp</value>
   </property>
   
   </configuration>
   ```

   [**hdfs-site.xml**]

   ```markdown
   <configuration>
   
   <property>
   <name>dfs.replication</name>
   <value>1</value> 
   </property>
   
   <property>
   <name>dfs.webhdfs.enabled</name>
   <value>true</value>
   </property>
   
   <property>
   <name>dfs.name.dir</name>
   <value>/usr/local/hadoop-1.2.1/name</value>
   </property>
   
   <property>
   <name>dfs.data.dir</name>
   <value>/usr/local/hadoop-1.2.1/data</value>
   </property>
   
   </configuration>
   ```

   [**mapred-site.xml**]

   ```markdown
   <configuration>
   
   <property>
   <name>mapred.job.tracker</name>
   <value>localhost:9001</value>
   </property>
   
   </configuration>
   ```

   [**hadoop.env.sh**]

   ```javascript
   9 export JAVA_HOME=/usr/local/jdk1.8.0
   10 export HADOOP_HOME_WARN_SUPPRESS="TRUE"
   ```

7. **hadoop namenode -format**

   * 안되면 reboot

8. **start-all.sh** ==> yes (처음에만 물어봄)

   * 멈출 때는 stop-all.sh (**poweroff 전 반드시 해주기**)

   * **JPS**를 이용해 확인

     4384 TaskTracker
     4178 SecondaryNameNode
     4258 JobTracker
     4038 DataNode
     4518 Jps
     3919 NameNode

9. firefox에서 **localhost:50070**으로 접속하여 확인

   

#### 제대로 설치되지 않는다면

* Q) `ssh servername` 입력시 password를 작성하라고 합니다.

  A)  **cd .ssh**로 이동하여 **rm id***을 실행합니다.

* Q) jps를 입력했는데 제대로 나오지 않아요.

  A) **cd /usr/local/hadoop-1.2.1**로 이동하여 다음을 실행합니다.

  * ```bash 
    rm -rf name
    rm -rf data
    rm -rf tmp
    ```

  * 이후 **vim /etc/profile**과  **cd /usr/local/hadoop-1.2.1/conf**의 파일들을 확인하고 다시 **hadoop namenode -format**을 실행합니다.



### HADOOP PRACTICE

* 글자수 세기 test

  * cd /usr/local/hadoop-1.2.1로 간다 (README.txt 사용)

  * hadoop fs -mkdir /test

    hadoop fs -put README.txt /test

    hadoop jar hadoop-examples-1.2.1.jar wordcount /test /output

---



## HADOOP 사용법

### 완전 분산 처리 환경

mainserver, secondserver, dataserver을 운용한다 가정한다

1. **vim /etc/hosts** 에 각 서버 IP와 name 입력

2. **ls .ssh** 확인

3. **ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa** 로 아이디 생성하고 

   **ssh-copy-id -i /root/.ssh/id_dsa.pub root@secondserver**

   **ssh-copy-id -i /root/.ssh/id_dsa.pub root@dataserver**

4. **ssh secondserver**, **ssh dataserver** 이용하여 접근이 되는지 확인해본다

5. jdk를 원격으로 설치한다

   ```css
   scp /etc/hosts root@secondserver:/etc/hosts
   
   scp ./jdk-8u261-linux-x64.tar.gz root@secondserver:/root
   
   scp /usr/bin/java root@secondserver:/usr/bin/java
       
   ssh root@secondserver tar xvf /root/jdk* 사용해서 원격으로 압축 풀기
       
   ssh root@secondserver mv jdk1.8.0_261 jdk1.8.0
       
   ssh root@secondserver cp -r /root/jdk1.8.0 /usr/local
   ```

6. **cd /usr/local/hadoop-1.2.1/conf**를 수정한다

   * **[core-site.xml]**

     localhost -> mainserver

   * **[hdfs-site.xml]**

     replica를 1->2

   * **[mapred-site]**

     localhost -> mainserver

   * **vim masters**에 secondserver

     **vim slaves**에 secondserver, dataserver 추가한다

7. **cd /usr/local** 에서

   * tar cvfz hadoop.tar.gz ./hadoop-1.2.1
     * 압축 파일만 전송 가능하다
   * scp hadoop.tar.gz root@secondserver:/usr/local
   * ssh root@secondserver tar xvf /usr/local/hadoop.tar.gz

   을 차례로 실행한다

8. **hadoop namenode -format**을 하고 **start-all.sh**을 한다

9. **jps**를 입력하면 jps, jobtraker, namenode 가 나온다

10. http://mainserver:50070 가면 **livenodes**에 dataserver, secondserver가 들어있다

11. stop-all.sh 하면 연결된 모든 것들이 같이 해제된다.



#### 가상분산모드 해제

* **cd /usr/local/hadoop-1.2.1** 에서 name, data, tmp 삭제
  * **rm -rf name**, **rm -rf data**, **rm -rf tmp**
* **vim /etc/hosts** 에서 **mainserver**만 남기자
* **cd /usr/local/hadoop-1.2.1/conf**
* **vim masters** ==> localhost
* **vim slaves** ==> localhost
* **vim hdfs-site.xml** 에서 replication을 1로
* **cd /usr/local/hadoop-1.2.1**에서 **hadoop namenode -format **하고 
* **ls** 이용해 **name 폴더** 생기는지 확인
* **start-all.sh** 하고 **jps**
* namenode, datanode, jobtracker, tasktracker, secondarynamenode,  jps 가 전부 뜨는지 확인



### HADOOP 명령어

* **hadoop fs -ls /usr** 
  * 아래에 뭐가 있는지 확인한다
* **hadoop fs -lsr /usr**
  * 하위의 모든 파일.폴더를 확인
* **hadoop fs -du /** 
  *  용량 확인

* **hadoop fs -get /output/part-r-00000 result.txt**
  * more result.txt 와 같이 읽을 수 있다
* **hadoop fs -getmerge /output resultall.txt**
  * /output에 저장된 문서를 resultall.txt로 합친다

* **hadoop fs -rmr /test **
  * r(디렉토리) rm(삭제)



# HIVE

## HIVE 설치

하둡에 저장된 데이터를 쉽게 처리할 수 있는 데이터웨어하우스 패키지

### 설치 시작

1. MariaDB 설정

   ```mysql
   1. mysqladmin -u root password '111111'
      mysql -h localhost -u root -p
      USE  mysql;
      GRANT ALL ON *.* TO hive@'127.0.0.1' IDENTIFIED BY '111111';
      GRANT ALL ON *.* TO hive@'localhost' IDENTIFIED BY '111111';
      GRANT ALL ON *.* TO hive@'192.168.111.120' IDENTIFIED BY '111111';
      GRANT ALL ON *.* TO hive@'hadoop' IDENTIFIED BY '111111';
      //mysql -h localhost -u hive -p  (작동 안됨)
      CREATE DATABASE hive_db;
      
   //hive_db database 생성
   //hive 계정 및 비번 111111
   ```

2. **wget https://archive.apache.org/dist/hive/hive-1.0.1/apache-hive-1.0.1-bin.tar.gz**

3. **tar xvf apache-hive-1.0.1-bin.tar.gz**

4. **mv apache-hive-1.0.1-bin hive**

5. **cp -r hive /usr/local**

6. **vim /etc/profile**

   ```bash
   JAVA_HOME=/usr/local/jdk1.8.0
   CLASSPATH=/usr/local/jdk1.8.0/lib
   HADOOP_HOME=/usr/local/hadoop-1.2.1
   HIVE_HOME=/usr/local/hive
   export JAVA_HOME CLASSPATH HADOOP_HOME HIVE_HOME
   PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HIVE_HOME/bin:.:$PATH
   ```

7. [카페](https://cafe.naver.com/multiiot2020)에서 "HIVE 설치" 게시물에 있는 **mariadb-java-client-1.3.5.jar** 파일 다운로드

8. 라이브러리로 jar파일 이동 

   **cp mariadb-java-client-1.3.5.jar /usr/local/hive/lib**

9. **vim /usr/local/hive/conf/hive-site.xml ** 에 다음을 기입

   ```bash
   <?xml version="1.0"?>
   <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
   <configuration>
       <property>
           <name>hive.metastore.local</name>
           <value>false</value>
           <description>controls whether to connect to remove metastore server or open a new metastore server in Hive Client JVM</description>
       </property>
   
       <property>
           <name>javax.jdo.option.ConnectionURL</name>
           <value>jdbc:mariadb://localhost:3306/hive_db?createDatabaseIfNotExist=true</value>
           <description>JDBC connect string for a JDBC metastore</description>
       </property>
   
       <property>
           <name>javax.jdo.option.ConnectionDriverName</name>
           <value>org.mariadb.jdbc.Driver</value>
           <description>Driver class name for a JDBC metastore</description>
       </property>
   
       <property>
           <name>javax.jdo.option.ConnectionUserName</name>
           <value>hive</value>
           <description>username to use against metastore database</description>
       </property>
   
       <property>
           <name>javax.jdo.option.ConnectionPassword</name>
           <value>111111</value>
           <description>password to use against metastore database</description>
       </property>
   
   </configuration>
   ```

10. hive에 사용될 폴더와 파일 생성

    hadoop fs -mkdir /tmp
    hadoop fs -chmod 777 /tmp
    hadoop fs -mkdir /tmp/hive
    hadoop fs -chmod 777 /tmp/hive
    hadoop fs -mkdir /user/root/warehouse
    hadoop fs -chmod 777 /user/root/warehouse

11. reboot 후 **hive**를 입력하면 들어가진다 !

    * 주의 ** 설치 후 hostname을 변경하면 데이터가 바뀌어 접속이 안된다.



## HIVE PRACTICE

1. [네이버 카페](https://cafe.naver.com/multiiot2020) 에 올라온 HIVE 예시 게시물에서 **hdi-data.csv**를 다운로드

2. 다운로드 폴더에서 **mv hdi-data.csv hdi.csv**하고 **mv hdi.csv /root**

3. TABLE 작성

   ```mysql
   CREATE TABLE HDI(
       id INT, 
       country STRING, 
       hdi FLOAT, 
       lifeex INT, 
       mysch INT, 
       eysch INT, 
       gni INT) 
       ROW FORMAT 
       DELIMITED FIELDS 
       TERMINATED BY ',' 
       STORED AS TEXTFILE;
   ```

4. 테이블에 데이터 넣기

   ```mysql
   LOAD DATA LOCAL INPATH '/root/hdi.csv' OVERWRITE INTO TABLE HDI;
   	//OVERWRITE 해주지 않으면 다시 LOAD 할 때, 같은 데이터도 중복돼서 들어간다
   select * from hdi limit 5;
   	//테이블에 잘 들어갔는지 확인, 많은 데이터가 있기 때문에 limit을 둬서 빠르게 처리한다.
   ```



* **show tables;**
  * 만들어진 테이블을 확인할 수 있다 (오류날 때 한번씩 확인해주기)



## HIVE와 Java 연결

```bash 
hive --service hiveserver2
```

* hive가 java를 기다린다

* [네이버 카페](https://cafe.naver.com/multiiot2020)에서 hivelibs를 받아 eclipse에 external해준다
* src만 이용해 데이터를 받을 때는 spring/a01의 파일 참고한다
* 많은 데이터를 이용해 서로 다른 프로젝트인 app과 hive를 연결하고 log를 찍을 때는 spring/hive와 spring/app을 참고한다
  * 구조가 너무 복잡하기 때문에 다 작성해도 가독성이 떨어진다. 참고하는 것으로 하자
