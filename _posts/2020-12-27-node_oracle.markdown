---
layout: post
title:  "[Node] Node.js - 오라클 연동에 mybatis까지"
subtitle:   "Node에 오라클에 mybatis까지"
categories: dev
tags: node
---

Node에 오라클(추천하지 않지만)연동 , Mybatis까지 적용해보자.

## 1.오라클 연동

**오라클**은 **Node**에 연동하기 위한 인터페이스 라이브러리를 제공하지 않습니다.

오라클에서 제공하는 ojdbc를 다운받거나 인터페이스 역할을 하는 클라이언트 라이브러리를 다운 받아서 적용해야 합니다.


[참고 - Node - 오라클 연동 ](lts0606.tistory.com/183)

1. [npm install](https://www.npmjs.com/package/oracledb) install

```
npm install oracledb
```

2. [client install](https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html)

3. oracle client 환경변수
   
[참고 - oracle client 환경변수](https://justdo-heal.tistory.com/11)


4. 코드
```javascript
/* 오라클 DB 설정*/
var oracledb = require('oracledb');
var config = {
    user: "아이디",
    password: "비밀번호",
    connectString: "주소/xe"
}

/* app.js */
oracledb.getConnection(config, (err, conn) =>{
    todoWork(err, conn);
});

/*databse connect*/
function todoWork(err, connection) {
    if (err) {
        console.error(err.message);
        return;
    }
    connection.execute("select * from test", [], 
    function (err, result){
        if (err) {
            console.error(err.message);
            doRelease(connection);
            return;
        }
        console.log(result.metaData);  //테이블 스키마
        console.log(result.rows);  //데이터
        doRelease(connection);
    });
}    

function doRelease(connection) {
    connection.release(function (err) {
        if (err) {
            console.error(err.message);
        }
    });
}

```


## 2.mybatis

1. [npm install](https://www.npmjs.com/package/mybatis-mapper)
```
.npm install mybatis-mapper
```

2. mapper 작성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="fruit">  
  <select id="testBasic">
    SELECT
      name,
      category,
      price
    FROM
      fruits 
    WHERE
      category = 'apple'
  </select>
</mapper>
```


3. 소스코드

### config.js - 설정 파일
```javascript
module.exports = {
	server_port: "2000",
	
	db_user:"test",
	db_pwd:"test",
	db_connectString:"192.xxx.xxx.xxx:1521/xe",

	//라우터 설정
	route_info:[
			//메인화면1
			{file:'../src/routes/main', path:'/', method:'main', type:'get'},
			{file:'../src/routes/main', path:'/login', method:'login', type:'post'}
	],
	// Mybatis Mapper 설정
	mapper_info:[
		{mapperNm : 'mainMapper' , path: './src/mapper/mainMapper.xml'},
		{mapperNm : 'sampleMapper' , path: './src/mapper/sampleMapper.xml'},
	]
}

```

### app.js
```javascript
// 모듈로 분리한 데이터베이스 파일 불러오기
var database = require('./config/database');
database.databaseInit();
app.set("database" , database);
```

### database.js
```javascript

const mybatisMapper = require('mybatis-mapper');
const config = require('./config');
const oracledb = require('oracledb')

oracledb.autoCommit=true;

// 서버 실행시 mapperNm 중복 체크를 합니다.
var databaseInit = function(){
	var arr = config.mapper_info;
	for(var z=0 ; z<arr.length ; z++){
		for( var y=0 ; y<z ; y++){
			if(arr[z].mapperNm == arr[y].mapperNm){
				
				console.log("[Database Error] ======> Mapper Name Duplication Exception")
				process.exit();
				break;
			};
		} ;
	};
};

var database = async function( mapperNm , queryId ,param){
	var mapperPath = null;
	
	//Mapper Path 및 name을 확인합니다.
	for(var i=0 ; i < config.mapper_info.length ; i++){
		if( config.mapper_info[i].mapperNm == mapperNm){
			mapperPath = config.mapper_info[i].path;
		};
	};
	if( mapperPath == null ){
		console.log("[Database Error] ======>Mapper Path Null Exception ");
		return false;
	};
	let oraConnection;
	let result;
    try{
        oraConnection = await oracledb.getConnection({
			user: process.env.NODE_ORACLEDB_USER || config.db_user,
			password: process.env.NODE_ORACLEDB_PASSWOR || config.db_pwd,
			connectString: process.env.NODE_ORACLEDB_CONNECTIONSTRING || config.db_connectString
		});
		oracledb.outFormat = oracledb.OBJECT;
		let format = { language: 'sql', indent: ' ' }; //첫번째는 xml의 namespace, 두번째는 해당 xml id값, 세번째는 파라미터, 마지막은 포맷. 
		mybatisMapper.createMapper([mapperPath]);
		let query = mybatisMapper.getStatement(mapperNm, queryId, param, format);
		console.log( "[DB SQL]]  "+queryId+"   In    "+mapperNm+"   ============>" );
		console.log(query);
		result = await oraConnection.execute( query  );
		
    }catch (err) {
        console.error(err);
    }finally {
        if (oraConnection) {
            try {
                await oraConnection.close();
                console.log("end connection...");
            } catch (err) {
                console.error(err);
            }
        };
	};
	
	if( (result == null) && (result == undefined)){
		return [{}];
	};
	return result.rows
};
```

### call database
```javascript
var database = req.app.get('database');
var result = await database( 'mainMapper' , 'getLogin' , params );
```


