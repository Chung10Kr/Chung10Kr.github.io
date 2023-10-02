---
layout: post
title:  "[etc] 네이버 Sens SMS API로 문자 보내기"
subtitle:   "Sens SMS API"
categories: dev
tags: note
---

[네이버 클라우드 플랫폼](https://www.ncloud.com/) 에서 제공하는 Sens SMS API를 사용해서 SMS 보내기


-[참고 - 네이버 클라우드 플랫폼](https://www.ncloud.com/)


-[참고 - SMS API 참조서](https://apidocs.ncloud.com/ko/ai-application-service/sens/sms_v2/)



## 준비
- Access Key ID 
> (계정관리 -> 인증키관리 -> Access Key ID 확인)
- Secret Key  
> (계정관리 -> 인증키관리 -> Secret Key 확인)
- 서비스 아이디   
> (콘솔 -> Simple & Easy Notification Service -> 프로젝트 생성후 서비스 아이디 확인)
- 등록된 발신번호 
> (콘솔 -> Simple & Easy Notification Service -> SMS -> SMS Calling Number -> 발시번호 등록)


## 요청 URL
![img](https://chung10kr.github.io/assets/img/2021-01-22-1.PNG)


## 요청 Body
![img](https://chung10kr.github.io/assets/img/2021-01-22-2.PNG)


## 코드

```java

import org.json.*;
import java.util.Base64;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import okhttp3.*;

public class smsMessage{
    
    private static String projectId = "서비스 아이디";
    private static String accessKey = "Access Key ID";
    private static String secretKey = "Secret Key";
    
    private static String url = "/sms/v2/services/"+projectId+"/messages";
    private static String requestUrl = "https://sens.apigw.ntruss.com"+url;
    
    private static String timestamp = Long.toString(System.currentTimeMillis()); 
    private static String method = "POST";
    
    
    public static void main(String[] args) throws Exception {
        
            JSONObject bodytJson = new JSONObject();
            JSONObject toJson = new JSONObject();
            JSONArray toArr = new JSONArray();
            toJson.put("subject" , "제목");
            toJson.put("content" , "내용");
            toJson.put("to" , "등록된 발신번호");
            toArr.put(toJson);

            bodytJson.put("type" , "SMS");
            bodytJson.put("contentType" , "COMM");
            bodytJson.put("countryCode" , "82");
            bodytJson.put("from" , "01012345678");
            bodytJson.put("subject" , "제목");
            bodytJson.put("content" , "내용");
            bodytJson.put("messages" , toArr );
            String body = bodytJson.toString();
            String result2 = doPost(requestUrl , body);
            System.out.println(result2);
    }
    
    // OkHttp 통신
    public static String doPost(String requestURL , String jsonMessage) throws Exception {
        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder()
                .addHeader("x-ncp-apigw-timestamp", timestamp )
                .addHeader("x-ncp-iam-access-key", accessKey )
                .addHeader("x-ncp-apigw-signature-v2", makeSignature() )
                .url(requestURL)
                .post(RequestBody.create(MediaType.parse("application/json"), jsonMessage)) 
                .build();

        Response response = client.newCall(request).execute();  
        //출력
        String message = response.body().string();
        return message;
    };
    
    // Signature생성
    public static String makeSignature() throws Exception {

        String space = " ";		// one space
        String newLine = "\n";	// new line
        
        String message = new StringBuilder()
            .append(method)
            .append(space)
            .append(url)
            .append(newLine)
            .append(timestamp)
            .append(newLine)
            .append(accessKey)
            .toString();

        String encodeBase64String = null;
        SecretKeySpec signingKey = new SecretKeySpec(secretKey.getBytes("UTF-8"), "HmacSHA256");
        Mac mac = Mac.getInstance("HmacSHA256");
        mac.init(signingKey);
    
        byte[] rawHmac = mac.doFinal(message.getBytes("UTF-8"));
        encodeBase64String = Base64.getEncoder().encodeToString(rawHmac);

      return encodeBase64String;
    };
    
}
```