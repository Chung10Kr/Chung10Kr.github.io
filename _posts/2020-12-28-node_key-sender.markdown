---
layout: post
title:  "[Node] Node.js - 키보드 입력시키기"
subtitle:   "node-key-sender npm 사용"
categories: dev
tags: node
---
물리적 키보드 입력과 같은 동작시키기!

원격지 컴퓨터의 키보드 입력과 같은 동작이 필요했었습니다.


나쁜 의도가 아닌 데이터 입력을 위한 기능이였고


node-key-sender npm을 통해 해결한 내용을 포스팅 하도록 하겠습니다.

## 1.npm 설치

[npm install](https://www.npmjs.com/package/node-key-sender/v/1.0.10) 

```
npm install --save-dev node-key-sender
```

## 소스코드
```javascript
var ks = require('node-key-sender');
ks.sendKey('a');
//키보드의 a 를 누르는 이벤트를 발생시킨다.

var ks = require('node-key-sender');
ks.sendKeys(['a', 'b', 'c']);
// abc
var ks = require('node-key-sender');
ks.sendCombination(['control', 'shift', 'v']);
// control + shift + v 를 누른다.
```

여기까지는 [node-key-sender](https://www.npmjs.com/package/node-key-sender/v/1.0.10) 을 읽어만 봐도 알 수 있는 내용이다.

# 문제 발생

a, c 와 같은 단순한 문자는 바로 바로 입력이 가능하였지만


**#1234** , **@A-1213** 와 같은 문자를 입력하기에는 문제가 있었다.


sendCombination()으로 특수문자를 입력후 sendKey또는 sendKeys를 입력해도
```javascript
ks.sendCombination(['shift', '2']); // -> @
ks.sendKeys(['a', 'b', 'c']); // ->abc
/*
예상 결과값 : @abc
실제 결과값 : abC@
*/
```
문자 순서가 뒤죽박죽 되었다. 예를들어 **@A-1213** 가 **A-@1213** 이런식으로 결과는 항상 달랐다.

# 문제 해결 고민


모든 문자(특수문자 포함) 가 순서대로 키보드 입력이 되게 하려면


**ks.sendKeys( arr );**의 **arr** 의 배열 형식으로 넣어야 하고

#,@ 와 같은 특수문자에 매핑이 되어있는 값을 찾아야 한다.

# 문제 해결

1. 첫번째 node-key-sender 의 jar파일과 js 파일을 복사하여 custom한다.

[node-key-sender](https://github.com/garimpeiro-it/node-key-sender) 에서 제공하는 jar 파일과 .js 파일을 내 프로젝트 경로에 가져왔다.

2. js파일 수정

기존의 keyboardLayout을 재정의 

(재정의 하는 이유 : 프로젝트에 #,@,- 가 필요 하기 때문에) 

```javascript
//기존의 있던 keyboardLayout
var keyboardLayout = {
        '+': 'add',
        '-': 'subtract',
        ' ': 'space',
        '&': 'shift-ampersand',
        '*': 'shift-asterisk',
        '@': 'shift-at',
        '`': 'shift-back_quote space',
        '~': '@514 space',
        '^': 'shift-circumflex space',
        '\\': 'back_slash',
        '/': 'slash',
        '{': 'shift-braceleft',
        '}': 'shift-braceright',
        '[': 'open_bracket',
        ']': 'close_bracket',
        ':': 'shift-colon',
        ';': 'semicolon',
        ',': 'comma',
        '$': 'shift-dollar',
        '€': 'alt_gr-euro_sign',
        '=': 'equals',
        '!': 'shift-exclamation_mark',
        '(': 'shift-left_parenthesis',
        ')': 'shift-right_parenthesis',
        '#': 'shift-number_sign',
        '.': 'period',
        "'": 'quote',
        '"': 'shift-quotedbl',
        '_': 'shift-underscore',
        '|': 'shift-@92',
        '?': 'shift-@47'
    };

/* # , # 작동 을 한해서  keyboardLayout 수정 함*/
var keyboardLayout = {
	'~': 'shift-@192', 
	'-': 'subtract',
	'!': 'shift-1', 
	'@': 'shift-2', 
	'#': 'shift-3', 
	'$': 'shift-4', 
	'%': 'shift-5', 
	'^': 'shift-6', 
	'&': 'shift-7', 
	'*': 'shift-8', 
	'(': 'shift-9', 
	')': 'shift-0', 
	'_': 'shift-@45', 
	'+': 'shift-@61', 
	'{': 'shift-@91', 
	'}': 'shift-@93', 
	':': 'shift-@59', 
	'"': "shift-@222",
	'|': 'shift-@92', 
	'<': 'shift-@44', 
	'>': 'shift-@46', 
	'?': 'shift-@47', 
	'': '@192',
}

```

Jar파일 경로 수정

```javascript
var jarPath = path.join("Jar파일 경로",'lib', 'key-sender.jar');
var command = 'java -jar \"' + jarPath + '\" ' + arrParams.join(' ') + module.getCommandLineOptions();
// "Jar파일 경로"도 중요합니다. 각자 맞는 경로로 수정해주세요.
```

   
3. 테스트

```javascript

  var ks = require("../utils/sender"); 
  // .js파일을 재정의 했기 때문에 기존의 npm이 아닌 custom 한 js파일 require 한다. 

  var txt = (req.body.sendTxt || req.query.sendTxt) ; // 받은 텍스트

  var sendTxt = [];
  var tx = "";
  if( txt != undefined){
    
    for( var i=0 ; i < txt.length ; i++){
      //현재 우리 프로젝트는 @,#,-밖에 없기 때문에 
      if( txt.substr(i,1) == "@"){
        tx = "shift-2";
      }else if( txt.substr(i,1)  == "#"){
        tx ="shift-3";
      }else if( txt.substr(i,1)  == "-"){
        tx = "subtract";
      }else{
        tx = txt.substr(i,1);
      }
      sendTxt.push(  tx );
    };
    sendTxt.push( 'enter'); //마지막 엔터
    
    ks.sendKeys( sendTxt ); // 끝
  };
```



영어 까막눈이라 doc를 구석구석 찾아보지 못했습니다.


혹시 다른방법이 있다면 알려주시면 감사하겠습니다.

