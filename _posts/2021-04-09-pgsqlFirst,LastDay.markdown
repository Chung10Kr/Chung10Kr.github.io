---
layout: post
title:  "[PostgreSQL] 이번달의 첫번째 날과 마지막날 구하기 "
subtitle:   "firstday , lastday "
categories: dev
tags: note
---




### 이번달 첫번째 날(01일) , 마지막날(30,31일) 구하기.

SELECT 
cast(date_trunc('month',current_date) as date) as firstday, 
(date_trunc('MONTH', current_date) + INTERVAL '1 MONTH - 1 day')::date lastday;

