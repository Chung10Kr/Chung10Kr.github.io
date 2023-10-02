---
layout: post
title:  "[NotePad++] NotePad++에서 PL/SQL 작성 및 컴파일"
subtitle:   ""
categories: dev
tags: note
--- 


보통 PL/SQL, Table script을 복사해서 Toad나 DBeaver와 같은 툴에 붙여넣기하여 실행한다.

그럴떄마다 PL/SQL을 복붙하기 힘들떄

NotePad++에서 바로 sql파일을 실행 하면 쉽게 컴파일 할 수 있다.





# 1.오라클 클라이언트 설치

- 오라클 클라이언트 설치
- tnsnames.ora 설정하기

# 2. NotePad++ 설치

- NotePad++ 설치


# 3. NotePad++ 플러그인 설치

- Explorer
- Npp Converter
- NppExec
- NppExport


# 4. NotePad++ 셋팅

- F6 으로 Execute 실행
- Command(s):에 아래 명령 입력 (User, Pwd, Sid는 알아서 입력) 후 save

### sql파일 실행후 exit 하는 명령어
```
set ORA_USER=dev01
set ORA_PASS=dev01!
set ORA_SID=orcl

npp_save
cmd /c copy /y "$(CURRENT_DIRECTORY)\$(FILE_NAME)" "$(SYS.TEMP)\$(FILE_NAME)" >nul 2>&1
cmd /c echo. >> "$(SYS.TEMP)\$(FILE_NAME)"
cmd /c echo exit >> "$(SYS.TEMP)\$(FILE_NAME)"
sqlplus -l $(ORA_USER)/$(ORA_PASS)@$(ORA_SID) @"$(SYS.TEMP)\$(FILE_NAME)"
cmd /c del "$(SYS.TEMP)\$(FILE_NAME)"
```


* 만약에 패스워드에 @가 들어갈 경우 -> 쌍따움표



### sql파일 실행후 exit 안하는 명령어
```
set ORA_USER=dev01
set ORA_PASS=dev01!
set ORA_SID=orcl

npp_save
cd "$(CURRENT_DIRECTORY)"
sqlplus -l $(ORA_USER)/$(ORA_PASS)@$(ORA_SID)
```


# 5. F6으로 Execute

notepad++에서 sql파일을 실행 할 수 있다.

