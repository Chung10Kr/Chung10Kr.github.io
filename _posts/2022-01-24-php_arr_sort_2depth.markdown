---
layout: post
title:  '[PHP] 2차원 배열 정렬 by value'
subtitle:   ""
categories: dev
tags: note
--- 
 

 PHP 2차원 배열 값으로 정렬하기
 
 
 # Code
 ```php
 function arr_sort( $array, $key, $sort ){
  $keys = array();
  $vals = array();
  foreach( $array as $k=>$v ){
    $i = $v[$key].'.'.$k;
    $vals[$i] = $v;
    array_push($keys, $k);
  }
  unset($array);

  if( $sort=='asc' ){
    ksort($vals);
  }else{
    krsort($vals);
  }
  
  $ret = array_combine( $keys, $vals );

  unset($keys);
  unset($vals);
  
  return $ret;
}

 ```


# Sample

 ```php

$arr = array(
    array('name' => '홍길동', 'age' => '30')
  , array('name' => '임세준', 'age' => '25')
  , array('name' => '히나노', 'age' => '24')
  , array('name' => '김광현', 'age' => '33')
  , array('name' => '류현진', 'age' => '34')
);

//배열 내 age 값 기준으로 오름차순으로 정렬한다
$result = arr_sort( $arr, 'age' , 'asc' );

//배열 내 name 값 기준으로 내림차순으로 정렬한다
$result = arr_sort( $arr,'name', 'desc' );
 ```



