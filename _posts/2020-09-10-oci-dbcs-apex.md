---
title: "[OCI] DBCS + APEX 설치"
---

# [OCI] DBCS + APEX 설치
### 개요
* DBCS와 APEX가 동작하는 전체적인 구성은 아래 그림과 같습니다.  APEX의 구성요소가 Oracle DB에 설치가 되어 있어야 하고 Tomcat 또는 Jetty WAS로 구동되는 ORDS 를 통해 Web으로 접속하게 됩니다.
![diagram](/assets/images/apex_ords_diagram01.png)
* DBCS에서 APEX를 사용하기 위해 아래의 준비 과정이 필요합니다.
	* APEX 에서 사용할 사용자, 구성 요소 설치
	* ORDS 설치

### 환경설정

* 필요한 서버에 접속, 파일을 전송하기 위해 아래의 커맨드를 활용하실 수 있습니다.

```
# 서버 접속
ssh -i "private PEM file path" opc@public_IP_address
```

```
# 서버에 파일 전송
scp -i "private PEM file path" "./apex_file_path" opc@public_IP_address:~/
scp -i "private PEM file path" "./ords_file_path" opc@public_IP_address:~/
```

* DB에 sys로 접속하여 아래 작업을 수행해야 합니다. 이는 유저를 사용가능하도록 unlock, 패스워드 설정 등 작업을 수행하게 됩니다.
	* 처음 opc 유저로 접속하게 되면 ORACLE_HOME과 ORACLE_SID 설정이 없습니다. 아래와 같이 확인 하고 설정하시면 됩니다. 아래의 그림처럼 설치하려는 oracle path와 SID를 확인하여 설정하도록 합니다.
![]({{ 'assets/images/db_server_bashrc_config.png' | relative_url }})

### 참조사이트

* 테스트로 설치한 APEX 사이트: [link](http://150.136.125.158:8080/ords/f?p=4550:1:7259868572154:::::){:target="_blank"}
