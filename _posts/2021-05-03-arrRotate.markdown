---
layout: post
title:  "[etc] 페이지 교체 알고리즘"
subtitle:   ""
categories: dev
tags: etc
--- 

# FIFO (First In First Out)
가장 먼저 들어와서 가장 오래 있었던 페이지를 교체하는 기법

# LRU (Least Recently Used Algorithm)

가장 오랫동안 참조되지 않은 페이지를 교체하는 기법

```javascript

let 동물 =  ['척추동물', '어류', '척추동물', '무척추동물', '파충류', '척추동물', '어류', '파충류'];
let 자리 = 3;

 
function solution(동물 , 자리){
    let 의자 = [];
    let answer=0;

    for( var 개별동물 of 동물 ){
        if( 의자.length < 3 )
        {
            if( 의자.includes(개별동물))
            {
                의자.splice( 의자.indexOf(개별동물) , 1)
                의자.push(개별동물)
            }
            else
            {
                의자.push( 개별동물 )
            }
        }
        else
        {
            if( 의자.includes(개별동물))
            {
                의자.splice( 의자.indexOf(개별동물) , 1)
                의자.push(개별동물)
            }
            else
            {
                의자.shift()
                의자.push( 개별동물 )
            }
        }
        console.log(의자)
    };
};
solution(동물,자리);
/*
1회 [ '척추동물' ]
2회 [ '척추동물', '어류' ]
3회 [ '어류', '척추동물' ]
4회 [ '어류', '척추동물', '무척추동물' ]
5회 [ '척추동물', '무척추동물', '파충류' ]
6회 [ '무척추동물', '파충류', '척추동물' ]
7회 [ '파충류', '척추동물', '어류' ]
8회 [ '척추동물', '어류', '파충류' ]

배열의 0번째 값은 가장 오랫동안 참조되지 않은 값임..
*/



```
# LFU (Least Frequently Used)

사용 빈도가 가장 적은 페이지를 교체하는 기법
