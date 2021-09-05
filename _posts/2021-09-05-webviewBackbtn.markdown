---
layout: post
title:  "[Android] 웹뷰에서 뒤로가기 "
subtitle:   ""
categories: dev
tags: error
--- 





단말기 뒤로가기할떄 웹뷰에서 뒤로가기




```java

@Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if ((keyCode == KeyEvent.KEYCODE_BACK)) {
            // 웹뷰 History상 이전 페이지가 있을 경우
            if(mWebView.canGoBack()){
                mWebView.goBack(); // 뒤로가기
                return true;
            }
            // 없을 경우 앱 종료 전 Toast로 물어보기
            else{
                // 토스트메세지 출력
                if (System.currentTimeMillis() > backKeyPressedTime + 2000) {
                    backKeyPressedTime = System.currentTimeMillis();
                    toast = Toast.makeText(this, "뒤로가기 버튼을 한번 더 누르시면 종료됩니다.", Toast.LENGTH_SHORT);
                    toast.show();
                    return true;
                }
                // 토스트메세지가 있는 상태에서 뒤로가기를 한번 더 누르면 앱 종료
                else if (System.currentTimeMillis() <= backKeyPressedTime + 2000) {
                    finish();
                    toast.cancel();
                }
            }
        }
        return super.onKeyDown(keyCode, event);
    }

```

[출처 - blog](https://blog.ysoft.kr/m/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%9B%B9%EB%B7%B0-%EB%92%A4%EB%A1%9C%EA%B0%80%EA%B8%B0-%EB%B2%84%ED%8A%BC-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)