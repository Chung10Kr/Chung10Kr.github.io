---
layout: post
title:  "[PostgreSQl]  PG/SQL Sample1 - Shell"
subtitle:   ""
categories: dev
tags: note
--- 




PG/SQL Shell Sample


- 접속한 DB확인, DB Instance변경
```
\c 
\c [DB Name] [Connection User]
```

- Database Instance 목록
```
\l
\list
```

- 접속한 DB Instance의 Table, Sequence, Function... 목록
```
\dt : table
\ds : sequence
\df : function
\dv : view
\du : user
\di : index 
....등등 더 있음

-- 옵션

\dt+ -> (+)를 붙히면 상세 정보까지 조회 가능
\dt users -> \dt [table name] 해당 테이블 정보 조회
```

- Command 실행, History 조회
```
\g : 방금 전에 실행했던 명령어 실행
\s : 이전에 실행했던 명령어 전체 List조회
```


- 도움말
```
\h : SQL command 관련 도움말
\? : SQL command 관련 도움말
```

- 외부 파일을 통한 Query 실행
```
\i test.sql : \i [File name]
```

- 종료
```
\q : 종료
```