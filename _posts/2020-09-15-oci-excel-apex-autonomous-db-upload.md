---
title: "[OCI] OAC 데이터를 Autonomous DB upload"
---

### [OCI] OAC 데이터를 Autonomous DB upload

OAC에서 사용하는 Data를 ADB에 올리기 위한 작업을 하기위한 방법 설명입니다.
OAC의 데이터를 엑셀로 내려받아 그 엑셀 파일을 SQL developer에서 올리는 방법에 대한 안내입니다.
#### 1. ADB에 올릴 데이터 준비
a. OAC 화면으로 이동, Data 테그를 선택하여 항목을 필터링 하고, ADB에 올리려고 하는 항목의 세부메뉴 항목을 클릭 한 후 "Download File" 버튼을 클릭하여 엑셀 형태로 파일을 저장합니다.
![]({{ 'assets/images/OAC_download_datasheet.png' | relative_url }})
#### 2. SQL developer 접속 및 데이터 업로드
a. OCI의 Autonomous data warehouse 항목으로 가서 tool 탭의 SQL developer 를 선택합니다. ADB의 접속정보를 이용해 로그인 합니다.
![]({{ 'assets/images/adw_sql_developer_connection.png' | relative_url }})

b.SQL workshop항목으로 이동, 메뉴를 클릭하고 Data Loading>Upload data into new... 항목을 선택해 줍니다. 그리고 1에서 내려받은 excel 파일을 선택해 줍니다.
![]({{ 'assets/images/SQL_DEVELOPER_DATA_LOADING01.png' | relative_url }})

c.DB에 들어갈 column명과 데이터 타입을 확인 합니다.
![]({{ 'assets/images/SQL_DEVELOPER_DATA_LOADING02.png' | relative_url }})

d.DB에 들어갈 데이터를 확인합니다.
![]({{ 'assets/images/SQL_DEVELOPER_DATA_LOADING03.png' | relative_url }})

위에 작업후 ADB에 데이터가 올라가게 됩니다.
