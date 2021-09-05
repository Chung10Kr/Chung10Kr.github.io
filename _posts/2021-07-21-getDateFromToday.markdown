---
layout: post
title:  "[Javascript] 오늘부터 n일 이후의 날짜 구하기"
subtitle:   ""
categories: dev
tags: lang
--- 

작업하다가 좋은거 하나 있어서 개인저장

오늘부터 n일 이후의 날짜 구하기

```javascript
/*
Param : 1,7,-7
*/
function getDateFromToday(i)
{
    today = new Date();
    ty = today.getFullYear();
    tm = today.getMonth()+1;
    td = today.getDate();
    if(tm<10) tm = "0" + tm;
    if(td<10) td = "0" + td;
    
    targetDay = new Date(today.valueOf()+(24*60*60*1000)*i);
        ty = targetDay.getFullYear();
        tm = targetDay.getMonth()+1;
        td = targetDay.getDate();
    if(tm<10) tm = "0" + tm;
    if(td<10) td = "0" + td;       
    return ty + tm + td;
}
/*
Param : 19930707
*/
function to_date(date_str)
{
    var yyyyMMdd = String(date_str);
    var sYear = yyyyMMdd.substring(0,4);
    var sMonth = yyyyMMdd.substring(4,6);
    var sDate = yyyyMMdd.substring(6,8);

    return new Date(Number(sYear), Number(sMonth)-1, Number(sDate));
}

var next7 = getDateFromToday(7);
var pre7 = getDateFromToday(-7);

console.log( to_date(next7) )

```
