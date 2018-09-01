---
title: Java RSA加密
date: 2018-07-06 13:55:01
tags: [java, rsa]
---

依赖Apache的`commons-codec-1.9.jar`：

```java
package utils;

import java.io.UnsupportedEncodingException;
import java.security.GeneralSecurityException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.Signature;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;

import javax.crypto.Cipher;

import org.apache.commons.codec.binary.Base64;

/**
 * RSA
 *
 * @date 2014-10-15
 * @version v1.1
 */
public class RSA {

    /** 签名算法 */
    public static final String SIGN_ALGORITHM = "SHA1WithRSA";

    /**
     * 私钥签名
     *
     * @param content
     *            要签名的数据
     * @param privateKey
     *            Base64格式的私钥
     * @return Base64格式的签名
     * @throws GeneralSecurityException
     */
    public static String sign(String content, String privateKey) throws GeneralSecurityException {
        PrivateKey priKey = getPrivateKey(privateKey);
        Signature signature = Signature.getInstance(SIGN_ALGORITHM);
        signature.initSign(priKey);
        signature.update(content.getBytes());
        return Base64.encodeBase64String(signature.sign());
    }

    /**
     * 验证签名
     *
     * @param content
     *            数据
     * @param sign
     *            Base64格式的签名
     * @param publicKey
     *            Base64格式的公钥
     * @return
     * @throws GeneralSecurityException
     */
    public static boolean verify(String content, String sign, String publicKey) throws GeneralSecurityException {
        PublicKey pubKey = getPublicKey(publicKey);
        Signature signature = Signature.getInstance(SIGN_ALGORITHM);
        signature.initVerify(pubKey);
        signature.update(content.getBytes());
        return signature.verify(Base64.decodeBase64(sign));
    }

    /**
     * 用公钥进行加密
     *
     * @param content
     *            明文
     * @param publicKey
     *            base64编码的公钥字符串
     * @return base64编码的密文
     * @throws UnsupportedEncodingException
     * @throws GeneralSecurityException
     */
    public static String encrypt(String content, String publicKey) throws UnsupportedEncodingException,
            GeneralSecurityException {
        PublicKey pubKey = getPublicKey(publicKey);
        Cipher cipher = Cipher.getInstance("RSA");
        byte[] byteContent = content.getBytes("utf-8");
        cipher.init(Cipher.ENCRYPT_MODE, pubKey);
        byte[] result = cipher.doFinal(byteContent);
        return Base64.encodeBase64String(result);
    }

    /**
     * 用私钥进行解密
     *
     * @param content
     *            base64编码的密文
     * @param privateKey
     *            base64编码的私钥字符串
     * @return 明文
     * @throws GeneralSecurityException
     */
    public static String decrypt(String content, String privateKey) throws GeneralSecurityException {
        PrivateKey priKey = getPrivateKey(privateKey);
        Cipher cipher = Cipher.getInstance("RSA");
        byte[] byteContent = Base64.decodeBase64(content);
        cipher.init(Cipher.DECRYPT_MODE, priKey);
        byte[] result = cipher.doFinal(byteContent);
        return new String(result);
    }

    /**
     * 将公钥字符串转换为PublicKey实例
     *
     * @param publicKey
     *            公钥base64编码的字符串
     * @return PublicKey实例
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     */
    private static PublicKey getPublicKey(String publicKey) throws GeneralSecurityException {
        KeyFactory keyFactory = KeyFactory.getInstance("RSA");
        byte[] encodedKey = Base64.decodeBase64(publicKey);
        return keyFactory.generatePublic(new X509EncodedKeySpec(encodedKey));
    }

    /**
     * 将私钥字符串转换为PrivateKey实例
     *
     * @param privateKey
     *            私钥base64编码的字符串
     * @return PrivateKey实例
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeySpecException
     */
    private static PrivateKey getPrivateKey(String privateKey) throws GeneralSecurityException {
        PKCS8EncodedKeySpec priPKCS8 = new PKCS8EncodedKeySpec(Base64.decodeBase64(privateKey));
        KeyFactory keyf = KeyFactory.getInstance("RSA");
        return keyf.generatePrivate(priPKCS8);
    }
}
```
