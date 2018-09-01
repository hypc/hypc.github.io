---
title: Java AES加密
date: 2018-07-06 13:56:59
tags: [java, aes]
---

依赖Apache的`commons-codec-1.9.jar`：

```java
package utils;

import java.io.UnsupportedEncodingException;
import java.security.GeneralSecurityException;
import java.security.SecureRandom;

import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;

import org.apache.commons.codec.binary.Base64;

/**
 * AES
 *
 * @date 2014-11-13
 * @version v1.0
 */
public class AES {

    /**
     * 加密
     *
     * @param content
     *            明文
     * @param password
     *            密码
     * @return base64编码密文
     * @throws GeneralSecurityException
     * @throws UnsupportedEncodingException
     */
    public static String encrypt(String content, String password) throws GeneralSecurityException,
            UnsupportedEncodingException {
        byte[] byteContent = content.getBytes("utf-8");
        return Base64.encodeBase64String(doFinal(byteContent, password, Cipher.ENCRYPT_MODE));
    }

    /**
     * 解密
     *
     * @param content
     *            base64编码的密文
     * @param password
     *            密码
     * @return 明文
     * @throws GeneralSecurityException
     */
    public static String decrypt(String content, String password) throws GeneralSecurityException {
        byte[] byteContent = Base64.decodeBase64(content);
        return new String(doFinal(byteContent, password, Cipher.DECRYPT_MODE));
    }

    private static byte[] doFinal(byte[] byteContent, String password, int mode) throws GeneralSecurityException {
        KeyGenerator kgen = KeyGenerator.getInstance("AES");
        kgen.init(128, new SecureRandom(password.getBytes()));
        SecretKey secretKey = kgen.generateKey();
        byte[] enCodeFormat = secretKey.getEncoded();
        SecretKeySpec key = new SecretKeySpec(enCodeFormat, "AES");
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(mode, key);
        return cipher.doFinal(byteContent);
    }
}
```
