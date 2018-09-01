---
title: Java MD5签名
date: 2018-07-06 13:57:45
tags: [java, md5]
---

依赖Apache的`commons-codec-1.9.jar`：

```java
package utils;

import java.io.UnsupportedEncodingException;

import org.apache.commons.codec.digest.DigestUtils;

/**
 * MD5签名
 *
 * @date 2014-11-14
 * @version v1.0
 */
public class MD5 {

    /**
     * 签名字符串
     *
     * @param text
     *            需要签名的字符串
     * @param key
     *            密钥
     * @return 签名结果
     * @throws UnsupportedEncodingException
     */
    public static String sign(String text, String key) throws UnsupportedEncodingException {
        text = text + key;
        return DigestUtils.md5Hex(text.getBytes("utf-8"));
    }

    /**
     * 签名字符串
     *
     * @param text
     *            需要签名的字符串
     * @param sign
     *            签名结果
     * @param key
     *            密钥
     * @return 验证结果
     * @throws UnsupportedEncodingException
     */
    public static boolean verify(String text, String sign, String key) throws UnsupportedEncodingException {
        text = text + key;
        String mysign = DigestUtils.md5Hex(text.getBytes("utf-8"));
        return mysign.equals(sign);
    }
}
```
