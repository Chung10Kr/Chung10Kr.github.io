---
layout: post
title:  "[Node] Node.js - Excel Download With Template"
subtitle:   "Excel Download With Template"
categories: dev
tags: node
---

xls Template 사용하여 Excel download wirh Template
 








## 현재 알고 있는 Excel download 방법 은 3가지 입니다.

1. 셀병합 , 셀 색변환등 엑셀작업을 Java,Node 단에서 작업해주는 방법 
-> 추후 엑셀 양식 변경시 대응하기 어렵다.
2. 그리드의 헤더만 엑셀 템플릿으로 만들어 리스트 데이터를 뿌려주는 방법
-> 헤더만 엑셀 템플릿으로 만들어 데이터만 뿌려주기 때문에 리스트데이터가 아닌것은 어렵다.
3. 셀병합등의 엑셀 작업도 엑셀 템플릿으로 만들고, 셀 위치를 지정하여 데이터를 뿌려줌
-> 보고서 양식 출력에 적당



## 구현

#### [npm install](https://www.npmjs.com/package/xlsx-template)


```
npm i xlsx-template
```


#### 구현


```javascript
var fs = require('fs');
var XlsxTemplate = require('xlsx-template');
var path = require('path');
var mime = require('mime');

//템플릿 명
var temp_name = "template01.xlsx";
//엑셀 데이터
var params={     
  extractDate: '20201127',
  revision: 2,
  dates: [new Date("1993-07-07"), new Date("2020-11-27") , new Date("2021-10-16") ],
  planData: [
      {
          name: "숫자도 가능",
          role: "테스트",
          days: [8, 8, 4]
      }, {
          name: "한글 테스트도 잘 됨",
          role: "KOREA",
          days: [4, 4, 4]
      }, {
          name: "ENG OK",
          role: "Manage",
          days: [4, 4, 4]
      }
  ]
};


const rootPath = "root 경로"
fs.readFile(path.join(rootPath, 'xlsTemplate/template', temp_name ), function(err, data) {
    //템플릿 을 읽어 Set data
    var t = new XlsxTemplate(data);
    t.substitute(1, params );

    var newData = t.generate();
    // 작성된 엑셀 파일을 temp.xlsx로 저장한다.
    fs.writeFile('xlsTemplate/dump/temp.xlsx', newData, 'binary', function(err) {
        file = path.join(rootPath , 'xlsTemplate/dump' ,'temp.xlsx');
        try {
            if (fs.existsSync(file)) { // 파일이 존재하는지 체크
              var filename = path.basename(file); // 파일 경로에서 파일명(확장자포함)만 추출
              var mimetype = mime.getType(file); // 파일의 타입(형식)을 가져옴
              res.setHeader('Content-disposition', 'attachment; filename=' + filename); // 다운받아질 파일명 설정
              res.setHeader('Content-type', mimetype); // 파일 형식 지정
              
              //작성된 엑셀 파일 다운로드
              var filestream = fs.createReadStream(file);
              filestream.pipe(res);

            }else{
                res.send('해당 파일이 없습니다.');  
                return;
            };
        } catch (e) { // 에러 발생시
            console.log(e);
            res.send('파일을 다운로드하는 중에 에러가 발생하였습니다.');
            return;
        };
    });
});

```


#### [Excel Template Download](https://chung10kr.github.io/assets/img/template01.xlsx)


![img](https://chung10kr.github.io/assets/img/2020-12-31-1.PNG)
