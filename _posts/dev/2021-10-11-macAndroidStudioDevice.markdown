---
layout: post
title:  "[Mac, Android Studio]  Android Studio랑 단말기 무선 연결"
subtitle:   ""
categories: dev
tags: error
--- 

안드로이드 스마트폰과 맥 무선으로 연결하기


# WIFI 연결

1. 안드로이드 스마트폰과 맥을 동일한 WIFI에 연결해 준다.
2. 안드로이드 스마트폰에서 디버그모드를 키고 맥과 연결해놓음

# ADB(AndroidDebug Bridge)설정


1. 맥 터미널 열기
2. /Users/lcy/Library/Android/sdk/platform-tools 로 이동
3. ./adb version 으로 확인

```
lcy@ichunglyeol-ui-MacBookPro platform-tools % ./adb version                
Android Debug Bridge version 1.0.41
Version 31.0.3-7562133
Installed as /Users/lcy/Library/Android/sdk/platform-tools/./adb
```

4. ./adb devices 으로 맥에 연결되어 있는 안드로이드 스마트폰 확인

```
lcy@ichunglyeol-ui-MacBookPro platform-tools % ./adb devices
List of devices attached
05157df5c10fc102	device
```

unauthorized 라고 나오면 연결이 허용이 제대로 되지 않은 상태임
USB케이블 뻇다가 다시 연결 후 스마트폰에서 허용

5. ./adb tcpip port번호 으로 맥과 스마트폰이 연결할 port번호 설정
```
lcy@ichunglyeol-ui-MacBookPro platform-tools % ./adb tcpip 5775
restarting in TCP mode port: 5775
```

# 안드로이드 스마트폰과 맥 분리

USB케이블 뽑아주세요.

# 안드로이드 스마트폰 IP 확인

스마트폰 설정에 WIFI의 연결정보에서 IP확인

설정 -> 와이파이 -> 사용하고 있는 와이파이 클릭 -> IP주소 확인


# 안드로이드 스마트폰 네크워크 연결
  
확인한 스마트폰 IP와 adb로 설정한 port를 이용

1. ./adb connect IP:port번호 으로 연결

```
lcy@ichunglyeol-ui-MacBookPro platform-tools % ./adb connect 192.168.45.98:5775
failed to authenticate to 192.168.45.98:5775
lcy@ichunglyeol-ui-MacBookPro platform-tools % ./adb connect 192.168.45.98:5775
already connected to 192.168.45.98:5775
```

# 안드로이드 스튜디오에서 확인

유선으로 연결한것처럼 안스에도 단말기 가 나옴 

-끝-