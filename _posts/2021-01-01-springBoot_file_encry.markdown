---
layout: post
title:  "[Spring] 파일 암호화"
subtitle:   "Spring 파일 암호화"
categories: dev
tags: lang
---

어제 올린 포스팅한 파일 압로드 후 파일 암호화 필요.





```java

import java.io.*;
import java.security.Key;
 
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
 
public class FileCoder {
 
    private static final String algorithm = "AES";
    private static final String transformation = algorithm + "/ECB/PKCS5Padding";
 
    private Key key;
 
    public FileCoder(Key key) {
        this.key = key;
    }
 
    /**
     * <p>
     * 원본 파일을 암호화해서 대상 파일을 만든다.
     * </p>
     *
     * @param source
     *            원본 파일
     * @param dest
     *            대상 파일
     * @throws Exception
     */
    public void encrypt(File source, File dest) throws Exception {
        crypt(Cipher.ENCRYPT_MODE, source, dest);
    }
 
    /**
     * <p>
     * 원본 파일을 복호화해서 대상 파일을 만든다.
     * </p>
     *
     * @param source
     *            원본 파일
     * @param dest
     *            대상 파일
     * @throws Exception
     */
    public void decrypt(File source, File dest) throws Exception {
        crypt(Cipher.DECRYPT_MODE, source, dest);
    }
 
    /**
     * <p>
     * 원본 파일을 암/복호화해서 대상 파일을 만든다.
     * </p>
     *
     * @param mode
     *            암/복호화 모드
     * @param source
     *            원본 파일
     * @param dest
     *            대상 파일
     * @throws Exception
     */
    private void crypt(int mode, File source, File dest) throws Exception {
        Cipher cipher = Cipher.getInstance(transformation);
        cipher.init(mode, key);
        InputStream input = null;
        OutputStream output = null;
        try {
            input = new BufferedInputStream(new FileInputStream(source));
            output = new BufferedOutputStream(new FileOutputStream(dest));
            byte[] buffer = new byte[1024];
            int read = -1;
            while ((read = input.read(buffer)) != -1) {
                output.write(cipher.update(buffer, 0, read));
            }
            output.write(cipher.doFinal());
        } finally {
            if (output != null) {
                try {
                    output.close();
                } catch (IOException ie) {
                }
            }
            if (input != null) {
                try {
                    input.close();
                } catch (IOException ie) {
                }
            }
        }
    }
 
    public static void main(String[] args) throws Exception {
        String test = "123456789abcdefg";
 
        // 128비트의 키
//      SecretKeySpec key = new SecretKeySpec(toBytes("696d697373796f7568616e6765656e61", 16), algorithm);
        SecretKeySpec key = new SecretKeySpec(test.getBytes(), algorithm);
 
        FileCoder coder = new FileCoder(key);
        coder.encrypt(new File("C:/temp/asdf.txt"), new File("C:/temp/asdf_a.txt"));
        coder.decrypt(new File("C:/temp/asdf_a.txt"), new File("C:/temp/asdf_b.txt"));
    }
 
    /**
     * <p>
     * 문자열을 바이트배열로 바꾼다.
     * </p>
     *
     * @param digits
     *            문자열
     * @param radix
     *            진수
     * @return
     * @throws IllegalArgumentException
     * @throws NumberFormatException
     */
    public static byte[] toBytes(String digits, int radix) throws IllegalArgumentException, NumberFormatException {
        if (digits == null) {
            return null;
        }
        if (radix != 16 && radix != 10 && radix != 8) {
            throw new IllegalArgumentException("For input radix: \"" + radix + "\"");
        }
        int divLen = (radix == 16) ? 2 : 3;
        int length = digits.length();
        if (length % divLen == 1) {
            throw new IllegalArgumentException("For input string: \"" + digits + "\"");
        }
        length = length / divLen;
        byte[] bytes = new byte[length];
        for (int i = 0; i < length; i++) {
            int index = i * divLen;
            bytes[i] = (byte) (Short.parseShort(digits.substring(index, index + divLen), radix));
        }
        return bytes;
    }
 
}

```


[-퍼온 포스팅입니다. 감사합니다. ](https://yangyag.tistory.com/388)