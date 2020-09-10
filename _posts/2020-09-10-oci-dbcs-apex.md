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
##### # APEX 설정

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
  * APEX 파일은 DB서버로, ORDS 파일은 VM 서버로 파일 전송 후 각 파일을 원하는 위치로 이동시키고 압축을 풀어줍니다.

* DB에 sys로 접속하여 아래 작업을 수행해야 합니다. 이는 유저를 사용가능하도록 unlock, 패스워드 설정 등 작업을 수행하게 됩니다.
	* 처음 opc 유저로 접속하게 되면 ORACLE_HOME과 ORACLE_SID 설정이 없습니다. 아래와 같이 확인 하고 설정하시면 됩니다. 아래의 그림처럼 설치하려는 oracle path와 SID를 확인하여 설정하도록 합니다.
![]({{ 'assets/images/db_server_bashrc_config.png' | relative_url }})
  * 압축이 풀린 APEX 폴더에 sqlplus를 수행하고 sys 계정으로 접속합니다. 그리고 APEX를 설치하게 될 PDB 명칭을 확인하여 해당 세션의 container를 변경해 줍니다.
![]({{ 'assets/images/db_server_sqlplus01.png' | relative_url }})
  * 아래 커맨드를 수행해 줍니다.
```
# 
@apexins.sql sysaux sysaux temp /i/
```
```
# 
@apxchpwd.sql
```
```
# 
@apxchpwd.sql
```
  * 위의 APEX 스크립트 수행이 다 되었으면 아래 SQL 스크립트를 수행해줍니다.
```
# password reset
alter user apex_public_user identified by "your_password";
alter user apex_listener identified by "your_password";
alter user apex_rest_public_user identified by "your_password";
```
```
# grant session
grant create session to apex_public_user;
grant create session to apex_listener;
grant create session to apex_rest_public_user;
```
```
# unlock account
alter user ords_public_user account unlock;
alter user apex_public_user account unlock;
alter user apex_listener account unlock;
alter user apex_rest_public_user account unlock;
```
  * 필요에 따라 해당 유저의 패스워드 변경 기간을 무제한으로 설정합니다.
```
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```
  * 이제 ORDS(APEX)에서 접속할 때 사용할 PDB의 service name을 확인합니다.
![]({{ 'assets/images/db_server_pdb_service_name.png' | relative_url }})

##### # ORDS 설정
* JDK 8 이상을 준비합니다.
* ORDS 를 실행시킬 VM에 접속 합니다.
* ORDS 압축을 해제하고 아래 명령어를 수행해 줍니다.  이때 수행하는 폴더안에 ords 이름의 폴더명으로 설정정보가 저장됩니다.
* static 폴더에는 apex의 images 폴더의 path를 지정해야 합니다.
```
java -jar ./ords.war install advanced
```
![]({{ 'assets/images/vm_server_ords_config01.png' | relative_url }})
* 이후 ords를 Standalone으로 실행해 줍니다.
![]({{ 'assets/images/vm_server_ords_config02.png' | relative_url }})

### 실행
##### # APEX 웹 접속
* VM 의 public 주소에 지정한 port(default 8080)로 접속합니다.
![]({{ 'assets/images/apex_01.png' | relative_url }})
* 스크롤을 아래로 내려 Administration에 ADMIN 계정으로 접속 합니다.
![]({{ 'assets/images/apex_02.png' | relative_url }})
  * 여기에서 하위 워크스페이스, 그 워크스페이스의 관리자를 생성해주고 초기 화면으로 돌아가 해당 워크스페이스에 관리자로 접근합니다.
  * 이후 사용자 추가하고 APEX 개발을 하실 수 있습니다.

### 참조사이트

* 테스트로 설치한 APEX 사이트: [link](http://150.136.125.158:8080/ords/f?p=4550:1:7259868572154:::::){:target="_blank"}
* APEX Tips : Basic APEX Management: [link](https://oracle-base.com/articles/misc/apex-tips-basic-apex-management){:target="_blank"}
* 4 Installing Application Express and Configuring Oracle REST Data Services: [link](https://docs.oracle.com/cd/E59726_01/install.50/e39144/listener.htm#HTMIG29143){:target="_blank"}
* Administration Guide: [link](https://docs.oracle.com/en/database/oracle/application-express/19.1/aeadm/index.html){:target="_blank"}
* Oracle Apex 20.1 Installation on Oracle 12c Part 1 Apex: [link](https://www.youtube.com/watch?v=aUkO2Qf1UvA&t=825s){:target="_blank"}
* Oracle REST Data Services (ORDS) : Installation on Tomcat: [link](https://oracle-base.com/articles/misc/oracle-rest-data-services-ords-installation-on-tomcat){:target="_blank"}
* ORACLE Ords+Apex 18.1 to 18.2 upgrade: [link](https://dayhap.tistory.com/entry/ORACLE-OrdsApex-181-to-182-upgrade){:target="_blank"}
* Oracle Apex 20.1 Installation: [link](https://bigdataenthusiast.wordpress.com/2020/08/02/apex-20-1-installation/){:target="_blank"}
* Install Oracle REST Data Services 3.0.X in under 5 minutes: [link](https://blog.cdivilly.com/2015/03/11/install-ords-3.0.0){:target="_blank"}
