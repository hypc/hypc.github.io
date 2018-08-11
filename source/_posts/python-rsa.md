---
title: python rsa
date: 2018-08-11 10:06:39
tags: [python, rsa]
---

## 生成RSA公钥、私钥

```python
key = RSA.generate(2048)
private_key = key.exportKey().decode()
public_key = key.publickey().exportKey().decode()
```

## 签名与验签

```python
# 签名
h = SHA.new(message)
signer = Signature.new(RSA.importKey(private_key))
sign = base64.b64encode(signer.sign(h)).decode()
# 验签
sign = base64.b64decode(sign)
h = SHA.new(message)
verifier = Signature.new(RSA.importKey(public_key))
verify = verifier.verify(h, sign)
```

## 加密与解密

```python
# 加密
cipher = Cipher.new(RSA.importKey(public_key))
ciphertext = base64.b64encode(cipher.encrypt(message)).decode()
# 解密
ciphertext = base64.b64decode(ciphertext)
sentinel = Random.new().read(SHA.digest_size)
cipher = Cipher.new(RSA.importKey(private_key))
message = cipher.decrypt(ciphertext, sentinel).decode()
```

<!--more-->

## 完整代码如下

```python
# encoding: utf-8
import base64

from Crypto import Random
from Crypto.Cipher import PKCS1_v1_5 as Cipher
from Crypto.Hash import SHA
from Crypto.PublicKey import RSA
from Crypto.Signature import PKCS1_v1_5 as Signature


class RSAUtils(object):

    @classmethod
    def generate_keys(cls):
        """
        :return: private_key and public_key
        :rtype: (str, str)
        """
        key = RSA.generate(2048)
        private_key = key.exportKey().decode()
        public_key = key.publickey().exportKey().decode()
        return private_key, public_key

    @classmethod
    def sign(cls, message, private_key):
        """
        :type message: str | bytes
        :type private_key: str | bytes
        :rtype: str
        """
        if isinstance(message, str):
            message = message.encode()
        h = SHA.new(message)
        signer = Signature.new(RSA.importKey(private_key))
        return base64.b64encode(signer.sign(h)).decode()

    @classmethod
    def verify(cls, message, sign, public_key):
        """
        :type message: str | bytes
        :type sign: str | bytes
        :type public_key: str | bytes
        :rtype: bool
        """
        if isinstance(message, str):
            message = message.encode()
        sign = base64.b64decode(sign)
        h = SHA.new(message)
        verifier = Signature.new(RSA.importKey(public_key))
        return verifier.verify(h, sign)

    @classmethod
    def encrypt(cls, message, public_key):
        """
        :type message: str | bytes
        :type public_key: str | bytes
        :rtype: str
        """
        if isinstance(message, str):
            message = message.encode()
        cipher = Cipher.new(RSA.importKey(public_key))
        return base64.b64encode(cipher.encrypt(message)).decode()

    @classmethod
    def decrypt(cls, ciphertext, private_key):
        """
        :type ciphertext: str | bytes
        :type private_key: str | bytes
        :rtype: str
        """
        ciphertext = base64.b64decode(ciphertext)
        sentinel = Random.new().read(SHA.digest_size)
        cipher = Cipher.new(RSA.importKey(private_key))
        return cipher.decrypt(ciphertext, sentinel).decode()
```
