---
title: Cryptography
date: 2021-09-18 19:29:09
tags: [python, cryptography]
---

[Cryptography](https://github.com/pyca/cryptography) 是 Python 中的一个加密库，它只能在 `Python3.6+`、`PyPi3 7.2+` 上使用。

可以直接使用下面命令安装：

```bash
pip install cryptography
```

## 生成密钥对

```python
from cryptography.hazmat.primitives.asymmetric import rsa

private_key = rsa.generate_private_key(public_exponent=65537, key_size=2048)
public_key = private_key.public_key()
```

## 导出密钥对

可以生成多个格式的文件。

**导出私钥**：

```python
from cryptography.hazmat.primitives.serialization import Encoding, PrivateFormat, NoEncryption

for encoding in Encoding:
    for fmt in PrivateFormat:
        try:
            # 无密码私钥
            print(encoding, fmt, private_key.private_bytes(encoding, fmt, NoEncryption()))
        except ValueError:
            pass
```

<!--more-->

执行结果如下：

```txt
Encoding.PEM PrivateFormat.PKCS8 b'-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDv4hmEYSthE3Uw\nROoyazwkp5XafO3ADbsdcQZNCx4r76ajJhMRlqdk+P2B3FYYDWSZPTLJssWMC8QW\net+IEaub9/v+FwssdX9/Axc8PGNpDWcuoWt+BStW/87yTpvXjMkJGzM8Se0PiYyt\nlLWx3UUandQayEN4FY6JwJIjy0ZLPXRFL1VvmHYEdMTSEj17HgcS5mFGIImrARIa\nXGVBHzzLDmFXd2CW/DK+vDXRRX+LvquYuseq1xEr9nPKNzU9FRDDU/IxmdjB2UQB\nXproON3O7+kL4D6FMfKQShBE39elryll2D9v+e2filAuSl6GOOkfXzhpYxvL77qm\nyUW2fnAnAgMBAAECggEARBPPmBEXhYJHJL66FDr4o5Jn5czEkFeVPcLAvgaktmVZ\nzj1U9g3iTbaYA02rpUHPxelnS3SPubHxIUwXuf8By86x1idmBWUHHN2cr3yX3c1u\n9f3birUe2p7YdU23zpFm0E3G2ZpFS76GjRCCDAs2vFoEQuGIvECp5hAfqUbcGSe3\n+osQCgQef8rZSG5aBM95+BAjCUkFi8jHuCVdeH6cc0VLS/D7v+lAkDWvvoIHLIme\nLrD13zfeFQa586esBM9qIAKmahuCtAnWrgEqPELmk6StK9s7B81dNy7ilBmkBTwB\n749Os46jxh7HZxYEm0cIq/Ee+TlFx0JEESaUwDNMiQKBgQD33fhwzxcsxirj9mL7\noPCqfBaFAsycfsHZ8OY5XxCtbgXRqFHnhj7Y6hUepLcs3/oBPS8zf1p0WkT+pvDJ\ncDuLrrPC5zGaN/OZwzSQbOG/3INJBhxhK4LZGMSadoligHksfNKGCS2xul91rN+k\nAdv/iqN9YL0UUS5jrvGwRtvT/QKBgQD3wREBi1/0t/yXaJt4z3Y7dw546EboacEs\ngTUEXjVqxB3t4gre+hHs3fe1PT3D+9LVKPI15YNk6RTHuzygH/4+WrdNfhtDipBp\n3rKwPOr5WrLBkn+RfteCyGl7GvzIRNq3HjeyyGieN03uZYnlGW8YedPDlyHIbqfp\nrL/lnY1D8wKBgA2uCYUoWM4Wzc0xDvt2QXIXUSLcKbDFait+GSa3cXMw7E9K6+JM\nTXGpUasUSivG3MRuvQkpkTN0u/QWAJoNgKvP44nxOpKZXe9xj5gc+kSdhf1kwfI2\n9YzHyioAOsrd7lIfPXs6THRPpe8XsGwb0imDXEySJz1U0aucvygMcRt9AoGBAMuN\n14ASV+tDQwfPDXWr1jMzNTPHe4K2aN085ydIk9C9gu2Qe2gJw7J+CGfjAh1EiEtU\nEfSQNm0xRz52qm/Q+V2XwOStSI8siExDiUJdOp1WlGmQCLmsojo0mN1pJekRETXE\nYPTFzZa4T5If4LTXOby9U2xufnYj3FeT9DIeSRNbAoGBANgrcQKDu5R8jukffJEh\nYw/lbFUF39bKvwQuTdQgkjSYWPoVKf/YzheCMgFOPkAFgMOVFdM+XueOnmIZ55EM\nouwzxN3DvS5Gj9w7Hez2igTIOjV05dkkDuU2zlJOCkgGWa7tRnCsRH5xsusKYCrB\n/4glxXtjQZxS4ZCAXLHS8j2u\n-----END PRIVATE KEY-----\n'
Encoding.PEM PrivateFormat.TraditionalOpenSSL b'-----BEGIN RSA PRIVATE KEY-----\nMIIEpAIBAAKCAQEA7+IZhGErYRN1METqMms8JKeV2nztwA27HXEGTQseK++moyYT\nEZanZPj9gdxWGA1kmT0yybLFjAvEFnrfiBGrm/f7/hcLLHV/fwMXPDxjaQ1nLqFr\nfgUrVv/O8k6b14zJCRszPEntD4mMrZS1sd1FGp3UGshDeBWOicCSI8tGSz10RS9V\nb5h2BHTE0hI9ex4HEuZhRiCJqwESGlxlQR88yw5hV3dglvwyvrw10UV/i76rmLrH\nqtcRK/Zzyjc1PRUQw1PyMZnYwdlEAV6a6Djdzu/pC+A+hTHykEoQRN/Xpa8pZdg/\nb/ntn4pQLkpehjjpH184aWMby++6pslFtn5wJwIDAQABAoIBAEQTz5gRF4WCRyS+\nuhQ6+KOSZ+XMxJBXlT3CwL4GpLZlWc49VPYN4k22mANNq6VBz8XpZ0t0j7mx8SFM\nF7n/AcvOsdYnZgVlBxzdnK98l93NbvX924q1Htqe2HVNt86RZtBNxtmaRUu+ho0Q\nggwLNrxaBELhiLxAqeYQH6lG3Bknt/qLEAoEHn/K2UhuWgTPefgQIwlJBYvIx7gl\nXXh+nHNFS0vw+7/pQJA1r76CByyJni6w9d833hUGufOnrATPaiACpmobgrQJ1q4B\nKjxC5pOkrSvbOwfNXTcu4pQZpAU8Ae+PTrOOo8Yex2cWBJtHCKvxHvk5RcdCRBEm\nlMAzTIkCgYEA9934cM8XLMYq4/Zi+6DwqnwWhQLMnH7B2fDmOV8QrW4F0ahR54Y+\n2OoVHqS3LN/6AT0vM39adFpE/qbwyXA7i66zwucxmjfzmcM0kGzhv9yDSQYcYSuC\n2RjEmnaJYoB5LHzShgktsbpfdazfpAHb/4qjfWC9FFEuY67xsEbb0/0CgYEA98ER\nAYtf9Lf8l2ibeM92O3cOeOhG6GnBLIE1BF41asQd7eIK3voR7N33tT09w/vS1Sjy\nNeWDZOkUx7s8oB/+Plq3TX4bQ4qQad6ysDzq+VqywZJ/kX7Xgshpexr8yETatx43\nsshonjdN7mWJ5RlvGHnTw5chyG6n6ay/5Z2NQ/MCgYANrgmFKFjOFs3NMQ77dkFy\nF1Ei3CmwxWorfhkmt3FzMOxPSuviTE1xqVGrFEorxtzEbr0JKZEzdLv0FgCaDYCr\nz+OJ8TqSmV3vcY+YHPpEnYX9ZMHyNvWMx8oqADrK3e5SHz17Okx0T6XvF7BsG9Ip\ng1xMkic9VNGrnL8oDHEbfQKBgQDLjdeAElfrQ0MHzw11q9YzMzUzx3uCtmjdPOcn\nSJPQvYLtkHtoCcOyfghn4wIdRIhLVBH0kDZtMUc+dqpv0Pldl8DkrUiPLIhMQ4lC\nXTqdVpRpkAi5rKI6NJjdaSXpERE1xGD0xc2WuE+SH+C01zm8vVNsbn52I9xXk/Qy\nHkkTWwKBgQDYK3ECg7uUfI7pH3yRIWMP5WxVBd/Wyr8ELk3UIJI0mFj6FSn/2M4X\ngjIBTj5ABYDDlRXTPl7njp5iGeeRDKLsM8Tdw70uRo/cOx3s9ooEyDo1dOXZJA7l\nNs5STgpIBlmu7UZwrER+cbLrCmAqwf+IJcV7Y0GcUuGQgFyx0vI9rg==\n-----END RSA PRIVATE KEY-----\n'
Encoding.PEM PrivateFormat.OpenSSH b'-----BEGIN OPENSSH PRIVATE KEY-----\nb3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcnNhAAAA\nAwEAAQAAAQEA7+IZhGErYRN1METqMms8JKeV2nztwA27HXEGTQseK++moyYTEZanZPj9gdxWGA1k\nmT0yybLFjAvEFnrfiBGrm/f7/hcLLHV/fwMXPDxjaQ1nLqFrfgUrVv/O8k6b14zJCRszPEntD4mM\nrZS1sd1FGp3UGshDeBWOicCSI8tGSz10RS9Vb5h2BHTE0hI9ex4HEuZhRiCJqwESGlxlQR88yw5h\nV3dglvwyvrw10UV/i76rmLrHqtcRK/Zzyjc1PRUQw1PyMZnYwdlEAV6a6Djdzu/pC+A+hTHykEoQ\nRN/Xpa8pZdg/b/ntn4pQLkpehjjpH184aWMby++6pslFtn5wJwAAA7jlnL0C5Zy9AgAAAAdzc2gt\ncnNhAAABAQDv4hmEYSthE3UwROoyazwkp5XafO3ADbsdcQZNCx4r76ajJhMRlqdk+P2B3FYYDWSZ\nPTLJssWMC8QWet+IEaub9/v+FwssdX9/Axc8PGNpDWcuoWt+BStW/87yTpvXjMkJGzM8Se0PiYyt\nlLWx3UUandQayEN4FY6JwJIjy0ZLPXRFL1VvmHYEdMTSEj17HgcS5mFGIImrARIaXGVBHzzLDmFX\nd2CW/DK+vDXRRX+LvquYuseq1xEr9nPKNzU9FRDDU/IxmdjB2UQBXproON3O7+kL4D6FMfKQShBE\n39elryll2D9v+e2filAuSl6GOOkfXzhpYxvL77qmyUW2fnAnAAAAAwEAAQAAAQBEE8+YEReFgkck\nvroUOvijkmflzMSQV5U9wsC+BqS2ZVnOPVT2DeJNtpgDTaulQc/F6WdLdI+5sfEhTBe5/wHLzrHW\nJ2YFZQcc3ZyvfJfdzW71/duKtR7anth1TbfOkWbQTcbZmkVLvoaNEIIMCza8WgRC4Yi8QKnmEB+p\nRtwZJ7f6ixAKBB5/ytlIbloEz3n4ECMJSQWLyMe4JV14fpxzRUtL8Pu/6UCQNa++ggcsiZ4usPXf\nN94VBrnzp6wEz2ogAqZqG4K0CdauASo8QuaTpK0r2zsHzV03LuKUGaQFPAHvj06zjqPGHsdnFgSb\nRwir8R75OUXHQkQRJpTAM0yJAAAAgQDYK3ECg7uUfI7pH3yRIWMP5WxVBd/Wyr8ELk3UIJI0mFj6\nFSn/2M4XgjIBTj5ABYDDlRXTPl7njp5iGeeRDKLsM8Tdw70uRo/cOx3s9ooEyDo1dOXZJA7lNs5S\nTgpIBlmu7UZwrER+cbLrCmAqwf+IJcV7Y0GcUuGQgFyx0vI9rgAAAIEA9934cM8XLMYq4/Zi+6Dw\nqnwWhQLMnH7B2fDmOV8QrW4F0ahR54Y+2OoVHqS3LN/6AT0vM39adFpE/qbwyXA7i66zwucxmjfz\nmcM0kGzhv9yDSQYcYSuC2RjEmnaJYoB5LHzShgktsbpfdazfpAHb/4qjfWC9FFEuY67xsEbb0/0A\nAACBAPfBEQGLX/S3/Jdom3jPdjt3DnjoRuhpwSyBNQReNWrEHe3iCt76Eezd97U9PcP70tUo8jXl\ng2TpFMe7PKAf/j5at01+G0OKkGnesrA86vlassGSf5F+14LIaXsa/MhE2rceN7LIaJ43Te5lieUZ\nbxh508OXIchup+msv+WdjUPzAAAAAAEC\n-----END OPENSSH PRIVATE KEY-----\n'
Encoding.DER PrivateFormat.PKCS8 b'0\x82\x04\xbe\x02\x01\x000\r\x06\t*\x86H\x86\xf7\r\x01\x01\x01\x05\x00\x04\x82\x04\xa80\x82\x04\xa4\x02\x01\x00\x02\x82\x01\x01\x00\xef\xe2\x19\x84a+a\x13u0D\xea2k<$\xa7\x95\xda|\xed\xc0\r\xbb\x1dq\x06M\x0b\x1e+\xef\xa6\xa3&\x13\x11\x96\xa7d\xf8\xfd\x81\xdcV\x18\rd\x99=2\xc9\xb2\xc5\x8c\x0b\xc4\x16z\xdf\x88\x11\xab\x9b\xf7\xfb\xfe\x17\x0b,u\x7f\x7f\x03\x17<<ci\rg.\xa1k~\x05+V\xff\xce\xf2N\x9b\xd7\x8c\xc9\t\x1b3<I\xed\x0f\x89\x8c\xad\x94\xb5\xb1\xddE\x1a\x9d\xd4\x1a\xc8Cx\x15\x8e\x89\xc0\x92#\xcbFK=tE/Uo\x98v\x04t\xc4\xd2\x12={\x1e\x07\x12\xe6aF \x89\xab\x01\x12\x1a\\eA\x1f<\xcb\x0eaWw`\x96\xfc2\xbe\xbc5\xd1E\x7f\x8b\xbe\xab\x98\xba\xc7\xaa\xd7\x11+\xf6s\xca75=\x15\x10\xc3S\xf21\x99\xd8\xc1\xd9D\x01^\x9a\xe88\xdd\xce\xef\xe9\x0b\xe0>\x851\xf2\x90J\x10D\xdf\xd7\xa5\xaf)e\xd8?o\xf9\xed\x9f\x8aP.J^\x868\xe9\x1f_8ic\x1b\xcb\xef\xba\xa6\xc9E\xb6~p\'\x02\x03\x01\x00\x01\x02\x82\x01\x00D\x13\xcf\x98\x11\x17\x85\x82G$\xbe\xba\x14:\xf8\xa3\x92g\xe5\xcc\xc4\x90W\x95=\xc2\xc0\xbe\x06\xa4\xb6eY\xce=T\xf6\r\xe2M\xb6\x98\x03M\xab\xa5A\xcf\xc5\xe9gKt\x8f\xb9\xb1\xf1!L\x17\xb9\xff\x01\xcb\xce\xb1\xd6\'f\x05e\x07\x1c\xdd\x9c\xaf|\x97\xdd\xcdn\xf5\xfd\xdb\x8a\xb5\x1e\xda\x9e\xd8uM\xb7\xce\x91f\xd0M\xc6\xd9\x9aEK\xbe\x86\x8d\x10\x82\x0c\x0b6\xbcZ\x04B\xe1\x88\xbc@\xa9\xe6\x10\x1f\xa9F\xdc\x19\'\xb7\xfa\x8b\x10\n\x04\x1e\x7f\xca\xd9HnZ\x04\xcfy\xf8\x10#\tI\x05\x8b\xc8\xc7\xb8%]x~\x9csEKK\xf0\xfb\xbf\xe9@\x905\xaf\xbe\x82\x07,\x89\x9e.\xb0\xf5\xdf7\xde\x15\x06\xb9\xf3\xa7\xac\x04\xcfj \x02\xa6j\x1b\x82\xb4\t\xd6\xae\x01*<B\xe6\x93\xa4\xad+\xdb;\x07\xcd]7.\xe2\x94\x19\xa4\x05<\x01\xef\x8fN\xb3\x8e\xa3\xc6\x1e\xc7g\x16\x04\x9bG\x08\xab\xf1\x1e\xf99E\xc7BD\x11&\x94\xc03L\x89\x02\x81\x81\x00\xf7\xdd\xf8p\xcf\x17,\xc6*\xe3\xf6b\xfb\xa0\xf0\xaa|\x16\x85\x02\xcc\x9c~\xc1\xd9\xf0\xe69_\x10\xadn\x05\xd1\xa8Q\xe7\x86>\xd8\xea\x15\x1e\xa4\xb7,\xdf\xfa\x01=/3\x7fZtZD\xfe\xa6\xf0\xc9p;\x8b\xae\xb3\xc2\xe71\x9a7\xf3\x99\xc34\x90l\xe1\xbf\xdc\x83I\x06\x1ca+\x82\xd9\x18\xc4\x9av\x89b\x80y,|\xd2\x86\t-\xb1\xba_u\xac\xdf\xa4\x01\xdb\xff\x8a\xa3}`\xbd\x14Q.c\xae\xf1\xb0F\xdb\xd3\xfd\x02\x81\x81\x00\xf7\xc1\x11\x01\x8b_\xf4\xb7\xfc\x97h\x9bx\xcfv;w\x0ex\xe8F\xe8i\xc1,\x815\x04^5j\xc4\x1d\xed\xe2\n\xde\xfa\x11\xec\xdd\xf7\xb5==\xc3\xfb\xd2\xd5(\xf25\xe5\x83d\xe9\x14\xc7\xbb<\xa0\x1f\xfe>Z\xb7M~\x1bC\x8a\x90i\xde\xb2\xb0<\xea\xf9Z\xb2\xc1\x92\x7f\x91~\xd7\x82\xc8i{\x1a\xfc\xc8D\xda\xb7\x1e7\xb2\xc8h\x9e7M\xeee\x89\xe5\x19o\x18y\xd3\xc3\x97!\xc8n\xa7\xe9\xac\xbf\xe5\x9d\x8dC\xf3\x02\x81\x80\r\xae\t\x85(X\xce\x16\xcd\xcd1\x0e\xfbvAr\x17Q"\xdc)\xb0\xc5j+~\x19&\xb7qs0\xecOJ\xeb\xe2LMq\xa9Q\xab\x14J+\xc6\xdc\xc4n\xbd\t)\x913t\xbb\xf4\x16\x00\x9a\r\x80\xab\xcf\xe3\x89\xf1:\x92\x99]\xefq\x8f\x98\x1c\xfaD\x9d\x85\xfdd\xc1\xf26\xf5\x8c\xc7\xca*\x00:\xca\xdd\xeeR\x1f={:LtO\xa5\xef\x17\xb0l\x1b\xd2)\x83\\L\x92\'=T\xd1\xab\x9c\xbf(\x0cq\x1b}\x02\x81\x81\x00\xcb\x8d\xd7\x80\x12W\xebCC\x07\xcf\ru\xab\xd63353\xc7{\x82\xb6h\xdd<\xe7\'H\x93\xd0\xbd\x82\xed\x90{h\t\xc3\xb2~\x08g\xe3\x02\x1dD\x88KT\x11\xf4\x906m1G>v\xaao\xd0\xf9]\x97\xc0\xe4\xadH\x8f,\x88LC\x89B]:\x9dV\x94i\x90\x08\xb9\xac\xa2:4\x98\xddi%\xe9\x11\x115\xc4`\xf4\xc5\xcd\x96\xb8O\x92\x1f\xe0\xb4\xd79\xbc\xbdSln~v#\xdcW\x93\xf42\x1eI\x13[\x02\x81\x81\x00\xd8+q\x02\x83\xbb\x94|\x8e\xe9\x1f|\x91!c\x0f\xe5lU\x05\xdf\xd6\xca\xbf\x04.M\xd4 \x924\x98X\xfa\x15)\xff\xd8\xce\x17\x822\x01N>@\x05\x80\xc3\x95\x15\xd3>^\xe7\x8e\x9eb\x19\xe7\x91\x0c\xa2\xec3\xc4\xdd\xc3\xbd.F\x8f\xdc;\x1d\xec\xf6\x8a\x04\xc8:5t\xe5\xd9$\x0e\xe56\xceRN\nH\x06Y\xae\xedFp\xacD~q\xb2\xeb\n`*\xc1\xff\x88%\xc5{cA\x9cR\xe1\x90\x80\\\xb1\xd2\xf2=\xae'
Encoding.DER PrivateFormat.TraditionalOpenSSL b'0\x82\x04\xa4\x02\x01\x00\x02\x82\x01\x01\x00\xef\xe2\x19\x84a+a\x13u0D\xea2k<$\xa7\x95\xda|\xed\xc0\r\xbb\x1dq\x06M\x0b\x1e+\xef\xa6\xa3&\x13\x11\x96\xa7d\xf8\xfd\x81\xdcV\x18\rd\x99=2\xc9\xb2\xc5\x8c\x0b\xc4\x16z\xdf\x88\x11\xab\x9b\xf7\xfb\xfe\x17\x0b,u\x7f\x7f\x03\x17<<ci\rg.\xa1k~\x05+V\xff\xce\xf2N\x9b\xd7\x8c\xc9\t\x1b3<I\xed\x0f\x89\x8c\xad\x94\xb5\xb1\xddE\x1a\x9d\xd4\x1a\xc8Cx\x15\x8e\x89\xc0\x92#\xcbFK=tE/Uo\x98v\x04t\xc4\xd2\x12={\x1e\x07\x12\xe6aF \x89\xab\x01\x12\x1a\\eA\x1f<\xcb\x0eaWw`\x96\xfc2\xbe\xbc5\xd1E\x7f\x8b\xbe\xab\x98\xba\xc7\xaa\xd7\x11+\xf6s\xca75=\x15\x10\xc3S\xf21\x99\xd8\xc1\xd9D\x01^\x9a\xe88\xdd\xce\xef\xe9\x0b\xe0>\x851\xf2\x90J\x10D\xdf\xd7\xa5\xaf)e\xd8?o\xf9\xed\x9f\x8aP.J^\x868\xe9\x1f_8ic\x1b\xcb\xef\xba\xa6\xc9E\xb6~p\'\x02\x03\x01\x00\x01\x02\x82\x01\x00D\x13\xcf\x98\x11\x17\x85\x82G$\xbe\xba\x14:\xf8\xa3\x92g\xe5\xcc\xc4\x90W\x95=\xc2\xc0\xbe\x06\xa4\xb6eY\xce=T\xf6\r\xe2M\xb6\x98\x03M\xab\xa5A\xcf\xc5\xe9gKt\x8f\xb9\xb1\xf1!L\x17\xb9\xff\x01\xcb\xce\xb1\xd6\'f\x05e\x07\x1c\xdd\x9c\xaf|\x97\xdd\xcdn\xf5\xfd\xdb\x8a\xb5\x1e\xda\x9e\xd8uM\xb7\xce\x91f\xd0M\xc6\xd9\x9aEK\xbe\x86\x8d\x10\x82\x0c\x0b6\xbcZ\x04B\xe1\x88\xbc@\xa9\xe6\x10\x1f\xa9F\xdc\x19\'\xb7\xfa\x8b\x10\n\x04\x1e\x7f\xca\xd9HnZ\x04\xcfy\xf8\x10#\tI\x05\x8b\xc8\xc7\xb8%]x~\x9csEKK\xf0\xfb\xbf\xe9@\x905\xaf\xbe\x82\x07,\x89\x9e.\xb0\xf5\xdf7\xde\x15\x06\xb9\xf3\xa7\xac\x04\xcfj \x02\xa6j\x1b\x82\xb4\t\xd6\xae\x01*<B\xe6\x93\xa4\xad+\xdb;\x07\xcd]7.\xe2\x94\x19\xa4\x05<\x01\xef\x8fN\xb3\x8e\xa3\xc6\x1e\xc7g\x16\x04\x9bG\x08\xab\xf1\x1e\xf99E\xc7BD\x11&\x94\xc03L\x89\x02\x81\x81\x00\xf7\xdd\xf8p\xcf\x17,\xc6*\xe3\xf6b\xfb\xa0\xf0\xaa|\x16\x85\x02\xcc\x9c~\xc1\xd9\xf0\xe69_\x10\xadn\x05\xd1\xa8Q\xe7\x86>\xd8\xea\x15\x1e\xa4\xb7,\xdf\xfa\x01=/3\x7fZtZD\xfe\xa6\xf0\xc9p;\x8b\xae\xb3\xc2\xe71\x9a7\xf3\x99\xc34\x90l\xe1\xbf\xdc\x83I\x06\x1ca+\x82\xd9\x18\xc4\x9av\x89b\x80y,|\xd2\x86\t-\xb1\xba_u\xac\xdf\xa4\x01\xdb\xff\x8a\xa3}`\xbd\x14Q.c\xae\xf1\xb0F\xdb\xd3\xfd\x02\x81\x81\x00\xf7\xc1\x11\x01\x8b_\xf4\xb7\xfc\x97h\x9bx\xcfv;w\x0ex\xe8F\xe8i\xc1,\x815\x04^5j\xc4\x1d\xed\xe2\n\xde\xfa\x11\xec\xdd\xf7\xb5==\xc3\xfb\xd2\xd5(\xf25\xe5\x83d\xe9\x14\xc7\xbb<\xa0\x1f\xfe>Z\xb7M~\x1bC\x8a\x90i\xde\xb2\xb0<\xea\xf9Z\xb2\xc1\x92\x7f\x91~\xd7\x82\xc8i{\x1a\xfc\xc8D\xda\xb7\x1e7\xb2\xc8h\x9e7M\xeee\x89\xe5\x19o\x18y\xd3\xc3\x97!\xc8n\xa7\xe9\xac\xbf\xe5\x9d\x8dC\xf3\x02\x81\x80\r\xae\t\x85(X\xce\x16\xcd\xcd1\x0e\xfbvAr\x17Q"\xdc)\xb0\xc5j+~\x19&\xb7qs0\xecOJ\xeb\xe2LMq\xa9Q\xab\x14J+\xc6\xdc\xc4n\xbd\t)\x913t\xbb\xf4\x16\x00\x9a\r\x80\xab\xcf\xe3\x89\xf1:\x92\x99]\xefq\x8f\x98\x1c\xfaD\x9d\x85\xfdd\xc1\xf26\xf5\x8c\xc7\xca*\x00:\xca\xdd\xeeR\x1f={:LtO\xa5\xef\x17\xb0l\x1b\xd2)\x83\\L\x92\'=T\xd1\xab\x9c\xbf(\x0cq\x1b}\x02\x81\x81\x00\xcb\x8d\xd7\x80\x12W\xebCC\x07\xcf\ru\xab\xd63353\xc7{\x82\xb6h\xdd<\xe7\'H\x93\xd0\xbd\x82\xed\x90{h\t\xc3\xb2~\x08g\xe3\x02\x1dD\x88KT\x11\xf4\x906m1G>v\xaao\xd0\xf9]\x97\xc0\xe4\xadH\x8f,\x88LC\x89B]:\x9dV\x94i\x90\x08\xb9\xac\xa2:4\x98\xddi%\xe9\x11\x115\xc4`\xf4\xc5\xcd\x96\xb8O\x92\x1f\xe0\xb4\xd79\xbc\xbdSln~v#\xdcW\x93\xf42\x1eI\x13[\x02\x81\x81\x00\xd8+q\x02\x83\xbb\x94|\x8e\xe9\x1f|\x91!c\x0f\xe5lU\x05\xdf\xd6\xca\xbf\x04.M\xd4 \x924\x98X\xfa\x15)\xff\xd8\xce\x17\x822\x01N>@\x05\x80\xc3\x95\x15\xd3>^\xe7\x8e\x9eb\x19\xe7\x91\x0c\xa2\xec3\xc4\xdd\xc3\xbd.F\x8f\xdc;\x1d\xec\xf6\x8a\x04\xc8:5t\xe5\xd9$\x0e\xe56\xceRN\nH\x06Y\xae\xedFp\xacD~q\xb2\xeb\n`*\xc1\xff\x88%\xc5{cA\x9cR\xe1\x90\x80\\\xb1\xd2\xf2=\xae'
```

```python
from cryptography.hazmat.primitives.serialization import Encoding, PrivateFormat, BestAvailableEncryption

for encoding in Encoding:
    for fmt in PrivateFormat:
        try:
            # 使用密码 `123456` 加密私钥，只有 PKCS8 支持使用密码加密私钥
            print(encoding, fmt, private_key.private_bytes(encoding, fmt, BestAvailableEncryption(b'123456')))
        except ValueError:
            pass
        except UnsupportedAlgorithm:
            pass
```

执行结果如下：

```txt
Encoding.PEM PrivateFormat.PKCS8 b'-----BEGIN ENCRYPTED PRIVATE KEY-----\nMIIFLTBXBgkqhkiG9w0BBQ0wSjApBgkqhkiG9w0BBQwwHAQIQTqcHtgJTvsCAggA\nMAwGCCqGSIb3DQIJBQAwHQYJYIZIAWUDBAEqBBDfyYznd/O6Rie3uvQUf/CsBIIE\n0C1LcQbGbisX0tlI/Hvb8RkY77L6iuTQZns25rtfE34Obu4Ilevnz7pbk0T3kBXI\npo5uReb+Hf/ffsgvpUyizXeqLUu5l0Hu2kQY1K1IwG8IikOQs0TM6KErBm7qndd8\n64V2rAr30V/14BVHAa19wReMkUe0mVVTz1yH9w8cjhxAfp0dtWFTk7Q0DoZETraZ\nLrW4Ncav7240odWHvPkZLK/VwKT+67IacGigTHlT9dxsHeornJent0tTPYFeLgmI\n8sENGQ9eEBwBDc9+8wbfQTUnhIvl9RaSwZj2R3NbImH+GTN6LP4WhDxFzp6mRbdP\nrXpYdCGep6/LeW15zUX8liQ5j9mT4/4nDzG33pyZtkO3dUfo/1A2GlXh642MPbZc\nnOCwO7COaeLuUYQ75ZWEpPoZGay4Rybax7bh5P19F3KI57CPFRvkFlWD8E1Nuz6n\n2oTsFTE1jKAeV230k6kR7+pDcP+bEbJMS9ZMsEXjNTknYKeKV03xnZg74ZObwVgl\nVchp2H5D5jWh7aCUSbYh0Rnh06GTZmfAgGFJlNcuCo7vp5W6G11jM5chlyKDJwg1\narOHe9+qCHan7rmRHnf0J7q9H8H6FV4Nv7fK0I2WQzBGXMm2Z6cN7JUOKaI6KKyK\nmsazPfbYb1iIqKCC+9w3tk8DpdQOvhBM1RVwvy6P8JKnIFnTHa3jAe/iYORfcKYG\nqXVHt7OEoRm0y3/zzDKyzKHGOf3xutmrZMZ3OnX8SgD/aUCJQDzJT3+JaiEm2ubM\nwbrZVZ3UGzLJB2/kdHhvn3UTwPhoHp7xvTuY6C+w/ay8GByArZRXXwWFHGj1PXur\n7JSM17CLKFgJEbNpCoLXgF//AoTzUzee2er+HmcKnThmTVx4FuuhzyvT1sB6Ob9E\nmsXofb4tiVEnnVyGb/BRH6GPtYiLTZ3fXLRzjzagOPTOkMz8L1oaFUkNNJ4UaTkz\nHb+g+cFg0JytO6tNRSqctRbuoYxC1NcGGisxwFuHMOVX5QipBwtSvFMv/WcaHa3s\ntikEEmiJaVNapWpj/f0TMQqpfBmyzAra3kxdfiKWHDQpIFc0KFNVwqiGuQgcesBl\nwFSappNnulv1voa9sm/alKJn+N0SZZ6l1S0uoOyradKodKSQUMAbhVzPOctlVsO6\nVcml3EzT0+1kRp9Zn9cqNr9lA8cU/eAzF5v4/JfS4VZVUlR85G6ypJ4JmTpCPAwC\npDqFH9S/Wc4smYkiUs1VD5Ln822oz02IocDPmllqHRIh4HP/4vUqu0snF1t9wGPH\ncBcbazZO9+iIdHR5rw4gqjA7/6xGTQQ4tLKfwulcMW9IgR+LINkWB8Ls1ueSXOXi\n01kKf8xghODG4aHNx4PICnjTtMXZlzzJI1DxkG1XI4tR+fX9msxR8GsAtOXdHjSj\nrOmoOtomhyfDrpS0auMUuMbbWrDE3WcF9WGBL7Ad5vs2Fi/l/UhVK72nyJYbs5vD\n8UAUaQaa6JcG4GK8qzF7FZKGW3ht3JoKk0PurWEskAxDlTGTpTxKpImrBvdjLeR+\n4KEfrhP8TK54Tgc+izR7lvSfBC2jyeKhcoDIAMql2Fu3kfBZ5nKk5/+gnxT7UsV8\nnx/uH9xs7YqbgykGAA+PwfIK71CYXh7eLyi52o39aXKf\n-----END ENCRYPTED PRIVATE KEY-----\n'
Encoding.PEM PrivateFormat.TraditionalOpenSSL b'-----BEGIN RSA PRIVATE KEY-----\nProc-Type: 4,ENCRYPTED\nDEK-Info: AES-256-CBC,106DE15E604344E7CD946BBD4475387F\n\nQdvIIdtEYoqsEN1bK9vUES2eE8C3+WyMS+hJHbrLxmxyD6W00b+sWrUH/b2Nl4+7\nUSN2r2m0T7wWRlWm6ahCLYB7qiGkuNX2HDrFdurvto4fluaBRW69YnOarVzu+dEw\nI/6yjAw19od8kouV0HPI8io2LyNHL2NE+8VttURzje2SGEGastsNHzoVMq+uh6pJ\njEDW3DztyhGANu+gpfx/DprM+7/molT+MhujHhAOPF6SZuF+K/pJ9sT6mw7mOL3g\nuWUCRmQuKgAls/CVLj9BJMYix6MLkB4nHmMfTx8HvIgvP4r5NHE1VOGzLyv9O1KD\n8EuPCoPFPMqa64dF6GgtsXPmsM/mMpUfLtqXxBi6+byqes5PgGofFhT2t3gA8c4t\nnwxrOblm3W9Ue13+AHQXfuyx+zVX+PAgy3SjRLTRYEBoNX887npdQlZYTdumjfgj\nxYeYrXBlBBVbFJGU+NTjl7BM6UoDtshFaE9B+GT7g7lz2H2tUmMU/34+Ch5m/idK\nmc/MiBAM5ncTHlKtxQIS6G5rQ1OFInl6ITr1EWS11C7kf7umz1mzGgAyp49ibiIG\nGQca3lbeLer22HGqRk6BXtkEoycHscfs1eLpuFUms++zbiLHw2jeESMr1r5AygxT\nFV7EG0U1nruO4aVxz4BjVLgxlyVaqmRMNA2N5+SAbvyVSh8XXDrRJW6/y5jD1jNQ\nxHm7yB6oQeiKLA++VM4JOTYyeV8d6BffUfEeALz2717vkrwGmjoZjszYmn5Tb3q5\nppSprrIf9k2AAnbn+Kqthhp95VpPXB6oyEnYrJ8+mEpzQgI6jbQYMf7Ct6OvXezj\n3VXTEebKVVXqC82pj3E0PnVijVmQ+duR1uptEiRMZIIe/kqddqx2g93GqFfXgTTi\nfy8ds9knPoKfTe2rjYFcdss6wO8Wff9nFXi2fVpFj6HDIGBURP0eOmbsAvUbfYCV\nL3A+f18iJF6KCmD7p7YwCBqCNyTM4zjGb9dZLN8Xe3QSH9j8noRtETvNmzSmRDof\n0p/LyGXpPTOKXZGZmcWUVoc+6VywvB7VQXamwfkn874ijnG8BzxpJP4Cd4DMJojI\n5z3btnjaI75N2Dg90kvRsHM2PNf71sk7Wrc1U2bbWgzn+UmdIIXZJ0XEyKMuBxr+\nVlSoOKjwIo3auTbhBSQb8vOSUmoc2BxIy+qzbZTXeUJDHGcSO2fMRKg+vI59+xiy\nuJLtT+6A87ZMi7qz83nwopddtmCkceL1H2019UfYAazUOScEWBvRdwGfT+UBnbHi\nrEzg4HPerFlVviCP0AghUnrAUWVXl/5RZl0VqK56avEDo3k+T1jRO2u9eRlY4LFe\nv37+/kIBBGJ3mMtGve0vkBTxLNkqPWvQgh+0JiNsVXIhqMFAoegxVnUOWNO5Pe9q\nOMhKwOWWtJ+9PeTJjzQIjNJG/sFysgVxICJI8E2ECiCDub3AvpK+UdaC1wfYVvvD\nGCyo+f7axTsGc/FYCGG6iPG2u25CPjuCTB8T7o+OCySvj6XMipau2fWByLQPhkrR\n9sXKjJMqIcHNWqgbjrwGaOi2XtlMsZOyGNdUruH+lyuphLLWefwJ+IEHj0xAo+tt\n-----END RSA PRIVATE KEY-----\n'
Encoding.DER PrivateFormat.PKCS8 b'0\x82\x05-0W\x06\t*\x86H\x86\xf7\r\x01\x05\r0J0)\x06\t*\x86H\x86\xf7\r\x01\x05\x0c0\x1c\x04\x08)\xecF\xdb\xe6:\xf0\xea\x02\x02\x08\x000\x0c\x06\x08*\x86H\x86\xf7\r\x02\t\x05\x000\x1d\x06\t`\x86H\x01e\x03\x04\x01*\x04\x10\x8e\xf8\x0c\xd4\xb8\x0e!\x0fZ\xc3\x84\xd3\xd7\x0c\x05\x0b\x04\x82\x04\xd0\x18Z\xcd\xad\x90P\x8a\xc4x\xb8\x11\x8dX#\xfc<\t\xa4@\xb1\x1a.\xf7\xac^?\xe3\x9b\x8d\'\xf6\xbb"&\xf0\t\x1bcY`ua\x99F\x861\xb7c\x00+\xd3\xa5n\x8b\xca\x14\xcc9\xc7[\xff\x05\xba\xe7\xbe\xba\x07\xee\x90\x0c\x8d\xf8K\x8f+\x0f\xe3\x17\xc1\xb7\x91he\xa5\xd4\xd9f\xf6\xf1#\xcf\x14il[\xfa\xf9\x88\xef\xa2\xc2\xe5\x0fv9\x0b\xda\xe72\x06"\x18\x9dg\xda\xb7\x8c\x93f\xff\x1a\x81M\x1fS\xec\x92`\x0f\x82\x83Q\xfe+r\xe5r>\xe1\xa9\xb7g \xf8+\x17\xc8!T\x912\x92\x16\x19^\xe5X\xbd\xaf\x9f!\x08\xa76w\xbak\x8e:\x00\xb3\xa9J\xf6\x97e\xa8{.\xe4\xc8\x96-\x03\x941\x1fp%\rS\x7f\xf8\xd6\x9f\xa3\x83\x1eM\xfd\x9dEa&\x1d\xe3\x86\x1e\xb6\x19\x99q\x8e>\x04\x93\xd6P\xa2\x91\xc7b;\xfa\x01\x86Y\x16\xbe9_\xcc\xc0\xc2\x8b\xa1\xc1k+\xc79i-5\x12q\x9e\t\x9d\x82\xdaU:4\xec\x9d\xd5\xe7\xc8Y\xb6? \x01\xb9\xe6eW&\x95\xae\xe9U\x90\x85\x1e\x0fV@\n\x85\x82\x88\x99\xd2\xef\x8f?\xcbUBO\xaex\x87\xf3\x07\xd5C\xd0;1X\xc0\xb3P\x91\x8b:\xb12\xfd0\xa7CJNi\xa0\xe0\xf7G\xe8\xe8\xbf\x86\xf5\xc0(\xdf\xf7\x0ex\x08\xbe\xd5\xa0)\xc1+\x98l\xd4\x9c\\|H\xb6u\x9b\x9a\xaf\x99\x9d\xb5\xede\x8ed3.\x9a36\xa2\x9b\xe1\xb2zb\xe4\xa5\x8d\x06Og\x9d\x851^\xf0y\x8e\xa7B\xaa\x97\xb7\xad\xec\x83\xda\x9a.\xd8\xaf3\xcbly\x13R\xa5\xb8\xb2o\xfb\x12\xfe\t|\xac\xa8\xb1g\xaeT\x0e\x05\x9fy\xa8\x03\x9b\xc2\xfe\xe2\x84\xday7\x86vnW\xfek\xd0\xaf\xb4\x9c\xf6\x8b\xc5\xd1\xf3\x12\x02\x9fG}\xfd\x83\xe7\xe6\x0c\xbc:\xc8W%\x19y\xb0\xa8`\x0b\xd4\xe8\x91\xbf?\x1d\x1c\xf2R\xb0\xc8\x95\xa9\xbemh%0\xe5\xf4CQky\x11e\x1c\xc9\x1f\x8e\xcbBg(\x01\x04N\xeeN\xc8s,:\x19\xb8\x93\xfe\xf5\xc3E\x14\x8e\xce\xb7K\xabO\xe52K\xf9\xf3L9\xc5\xf3AD@\xdd>!#$2!\xd7\x18]\xcb\xec]S)\xddw\xc9\xabI\x11-F\xb0\x88\x80\xa9\xf8\x008\x1e\x1aw\x02<F\x8a|N\xf1\x89`um\xc5\xe0\x9f\x89\x95\xce\x8e\x8b\xa4\xfb\x8a\xec\xc2H@z\xe4\x0f\x9f\xb3\xe6{\xa4\x9d\xfd0o#L^p\xce3_\xba\x1d>4\xeb\xc8\x98\xc5\xf35bB\r\x88\xdf\xf8\xe4\xbcE\xfcv\x90\xf9\x81\xa97\xf9SU\xf0\x9b\x82\xf5NY.\x06\xc5\x03\xc1C\xb9\x12\xd7\x05\x0c\x80\x12v^\xfc\n\xf4\'*\x03`\xb3x\x8fp7\xd7\xb14c\xe8N)a\x95z\x12\xcef\x1b\x84*o\xa1\x81\xfd\x9cC\xed?\xfa\xcd\x88]\xb8w\xdf\xf1F\r\xd9\x81\x91\xc5\xb4-f`mn\x8c*\x12\xb4B%\x957\x88\x84\x9a\xae\xae\x91\xc1!\xb2K\xee\xb0G\xd8G\x12\x91?\x8b\xc1\xaaI\xe2d\xe4\x87\xa43K\x82W\xefYwG\xbe\xb4\x10\x1c\xb4\xdf\x7fA\xfaJ?h\x9c\xcc\xa0G\xd8\x9aS\xae\x9f%\xf2\xd2\x18\x95\xec,\xfc)T\x91BB\xc1\xd97\'\xe7\xe6\xec3\xe0\xf3\xcb\x8dE\x95\x020\x0c\xe3n\xda:vk\xa1\xd9\x0b\x91\xd5=Y\xacA\x80\xb5\x00\xe9m\x11\xf1\xb2y"\xbav\xedQ\xc1\x89\xe6@d4zq%\xfc\xb7i\x1f\xf8\x13\xc5`\xbbV(\x87\x0e{&cF\x9d,e\xb4rDO\xec\xd7\x9cb\xadb\xbe\xa8\x883C\xf31~\x89z^@S\xbe\t\xa5\x00@W\xd2\x1cRLEX\xdcT\xbd&lA0\'\xaa\xd4coG\x82-Y.\x0b\x8b&\xb59\x87\x03\xbb\x80\xb4o\x11\xc9\xd8\xde\x07\x00\x9d\xf0\x91\x18\x9eu\xd6\xdd\x93\xe1\x18\xd7\xfb?\xfe\x18I\x89\xb5iZP%K\xf5N(X\x08\xa9\x81\x0fu\x84\xca\x10\xc1\xd1\x83\xeb\xd5mZ\xaaw|\x98\xcc\xf5#!\x1d_\x9d\x90\xf6\xe73\x9f\x9a\x1ao\xd1^\x02\'\x8a\x88M\xa7\xa5\x03\xe3\xed[g\xe6\\Z\xe0W&I\x11J\x83\x96KO\x87(\xd0\x99m\xa8\xce\x1e\xd9\x8f} <3\x10\xa9b4z\x88\x878\x9a\x07\n\xbc\xce\xe7z\x1a:\xa2t\xd16\x93\x19\xfa \x1dS\xd4\xc8\xba\xfe\x83N\x9c\xee\x92\xbd\xbd\xd0\xbc\xf0e\xf1\xcc\xb5\xd0\xe3=)~\xab\xc6\r7\x9eA\xe2\xc0\xa5\xc2h\x9c\xc3\x04|~\'\x86\xf53\x05\xa5\xf2[Q\x85\xce\xf4\xf3\x82\xb5@"r\x1a\xdcu\xc1\xce J\xae\xb1\xdeZ\xf9\xa1y5G\x80\x8f\xd5\xe0\xa3\x08)\xf8\x83\xe9\xc2\x8b\x8e\xadc\xaa\x1d\xa5\x0b\x87C\xec(\xc9bL\xe4`\xa7\x96"\xe1\xa9M2f\x16\xb2\xfb\xe6\xbd*0\x9f\xe0\xe51\xdbt\xa4@}\x80`\x0bs\xd1\xbb\xc1%u\x96h\x0b%1h\xa44\xdd\x18\xf9\xf6\xf2Z\xfa\x02\xa2W\xab0\xfd\xba\xeeX is'
```

**导出公钥**：

```python
from cryptography.hazmat.primitives.serialization import Encoding, PublicFormat

for encoding in Encoding:
    for fmt in PublicFormat:
        try:
            print(encoding, fmt, public_key.public_bytes(encoding, fmt))
        except ValueError:
            pass
```

执行结果如下：

```txt
Encoding.PEM PublicFormat.SubjectPublicKeyInfo b'-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7+IZhGErYRN1METqMms8\nJKeV2nztwA27HXEGTQseK++moyYTEZanZPj9gdxWGA1kmT0yybLFjAvEFnrfiBGr\nm/f7/hcLLHV/fwMXPDxjaQ1nLqFrfgUrVv/O8k6b14zJCRszPEntD4mMrZS1sd1F\nGp3UGshDeBWOicCSI8tGSz10RS9Vb5h2BHTE0hI9ex4HEuZhRiCJqwESGlxlQR88\nyw5hV3dglvwyvrw10UV/i76rmLrHqtcRK/Zzyjc1PRUQw1PyMZnYwdlEAV6a6Djd\nzu/pC+A+hTHykEoQRN/Xpa8pZdg/b/ntn4pQLkpehjjpH184aWMby++6pslFtn5w\nJwIDAQAB\n-----END PUBLIC KEY-----\n'
Encoding.PEM PublicFormat.PKCS1 b'-----BEGIN RSA PUBLIC KEY-----\nMIIBCgKCAQEA7+IZhGErYRN1METqMms8JKeV2nztwA27HXEGTQseK++moyYTEZan\nZPj9gdxWGA1kmT0yybLFjAvEFnrfiBGrm/f7/hcLLHV/fwMXPDxjaQ1nLqFrfgUr\nVv/O8k6b14zJCRszPEntD4mMrZS1sd1FGp3UGshDeBWOicCSI8tGSz10RS9Vb5h2\nBHTE0hI9ex4HEuZhRiCJqwESGlxlQR88yw5hV3dglvwyvrw10UV/i76rmLrHqtcR\nK/Zzyjc1PRUQw1PyMZnYwdlEAV6a6Djdzu/pC+A+hTHykEoQRN/Xpa8pZdg/b/nt\nn4pQLkpehjjpH184aWMby++6pslFtn5wJwIDAQAB\n-----END RSA PUBLIC KEY-----\n'
Encoding.DER PublicFormat.SubjectPublicKeyInfo b'0\x82\x01"0\r\x06\t*\x86H\x86\xf7\r\x01\x01\x01\x05\x00\x03\x82\x01\x0f\x000\x82\x01\n\x02\x82\x01\x01\x00\xef\xe2\x19\x84a+a\x13u0D\xea2k<$\xa7\x95\xda|\xed\xc0\r\xbb\x1dq\x06M\x0b\x1e+\xef\xa6\xa3&\x13\x11\x96\xa7d\xf8\xfd\x81\xdcV\x18\rd\x99=2\xc9\xb2\xc5\x8c\x0b\xc4\x16z\xdf\x88\x11\xab\x9b\xf7\xfb\xfe\x17\x0b,u\x7f\x7f\x03\x17<<ci\rg.\xa1k~\x05+V\xff\xce\xf2N\x9b\xd7\x8c\xc9\t\x1b3<I\xed\x0f\x89\x8c\xad\x94\xb5\xb1\xddE\x1a\x9d\xd4\x1a\xc8Cx\x15\x8e\x89\xc0\x92#\xcbFK=tE/Uo\x98v\x04t\xc4\xd2\x12={\x1e\x07\x12\xe6aF \x89\xab\x01\x12\x1a\\eA\x1f<\xcb\x0eaWw`\x96\xfc2\xbe\xbc5\xd1E\x7f\x8b\xbe\xab\x98\xba\xc7\xaa\xd7\x11+\xf6s\xca75=\x15\x10\xc3S\xf21\x99\xd8\xc1\xd9D\x01^\x9a\xe88\xdd\xce\xef\xe9\x0b\xe0>\x851\xf2\x90J\x10D\xdf\xd7\xa5\xaf)e\xd8?o\xf9\xed\x9f\x8aP.J^\x868\xe9\x1f_8ic\x1b\xcb\xef\xba\xa6\xc9E\xb6~p\'\x02\x03\x01\x00\x01'
Encoding.DER PublicFormat.PKCS1 b"0\x82\x01\n\x02\x82\x01\x01\x00\xef\xe2\x19\x84a+a\x13u0D\xea2k<$\xa7\x95\xda|\xed\xc0\r\xbb\x1dq\x06M\x0b\x1e+\xef\xa6\xa3&\x13\x11\x96\xa7d\xf8\xfd\x81\xdcV\x18\rd\x99=2\xc9\xb2\xc5\x8c\x0b\xc4\x16z\xdf\x88\x11\xab\x9b\xf7\xfb\xfe\x17\x0b,u\x7f\x7f\x03\x17<<ci\rg.\xa1k~\x05+V\xff\xce\xf2N\x9b\xd7\x8c\xc9\t\x1b3<I\xed\x0f\x89\x8c\xad\x94\xb5\xb1\xddE\x1a\x9d\xd4\x1a\xc8Cx\x15\x8e\x89\xc0\x92#\xcbFK=tE/Uo\x98v\x04t\xc4\xd2\x12={\x1e\x07\x12\xe6aF \x89\xab\x01\x12\x1a\\eA\x1f<\xcb\x0eaWw`\x96\xfc2\xbe\xbc5\xd1E\x7f\x8b\xbe\xab\x98\xba\xc7\xaa\xd7\x11+\xf6s\xca75=\x15\x10\xc3S\xf21\x99\xd8\xc1\xd9D\x01^\x9a\xe88\xdd\xce\xef\xe9\x0b\xe0>\x851\xf2\x90J\x10D\xdf\xd7\xa5\xaf)e\xd8?o\xf9\xed\x9f\x8aP.J^\x868\xe9\x1f_8ic\x1b\xcb\xef\xba\xa6\xc9E\xb6~p'\x02\x03\x01\x00\x01"
Encoding.OpenSSH PublicFormat.OpenSSH b'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDv4hmEYSthE3UwROoyazwkp5XafO3ADbsdcQZNCx4r76ajJhMRlqdk+P2B3FYYDWSZPTLJssWMC8QWet+IEaub9/v+FwssdX9/Axc8PGNpDWcuoWt+BStW/87yTpvXjMkJGzM8Se0PiYytlLWx3UUandQayEN4FY6JwJIjy0ZLPXRFL1VvmHYEdMTSEj17HgcS5mFGIImrARIaXGVBHzzLDmFXd2CW/DK+vDXRRX+LvquYuseq1xEr9nPKNzU9FRDDU/IxmdjB2UQBXproON3O7+kL4D6FMfKQShBE39elryll2D9v+e2filAuSl6GOOkfXzhpYxvL77qmyUW2fnAn'
```

## 导入密钥对

**导入私钥**：

```txt
from cryptography.hazmat.primitives.serialization import *

Encoding.PEM PrivateFormat.PKCS8 -> load_pem_private_key
Encoding.PEM PrivateFormat.TraditionalOpenSSL -> load_pem_private_key
Encoding.DER PrivateFormat.PKCS8 -> load_der_private_key
Encoding.DER PrivateFormat.TraditionalOpenSSL -> load_der_private_key
Encoding.PEM PrivateFormat.OpenSSH -> load_ssh_private_key
```

**导入公钥**：

```txt
from cryptography.hazmat.primitives.serialization import *

Encoding.PEM PublicFormat.SubjectPublicKeyInfo -> load_pem_public_key
Encoding.PEM PublicFormat.PKCS1 -> load_pem_public_key
Encoding.DER PublicFormat.SubjectPublicKeyInfo -> load_der_public_key
Encoding.DER PublicFormat.PKCS1 -> load_der_public_key
Encoding.OpenSSH PublicFormat.OpenSSH -> load_ssh_public_key
```

## 签名 & 验签

**签名过程**

1. A 计算消息 m 的 hash 值，记为 h
2. A 使用私钥对 h 进行加密，得到密文（签名）s
3. A 将消息 m 及签名 s 发送给 B

**验签过程**

1. B 计算消息 m 的 hash 值，记为 h
2. B 使用 A 的公钥解密 s，得到 H
3. 判断 h、H 是否相同，相同则验签通过

有两种签名方案：`PKCS1`、`EMSA-PSS`，hash 算法支持：`SHA1`、`SHA224`、`SHA256`、
`SHA384`、`SHA512`、`SHA3_224`、`SHA3_256`、`SHA3_384`、`SHA3_512`、`MD5`。

```python
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric.padding import PKCS1v15, PSS, MGF1

msg = b'hello world'

p = PKCS1v15()
signature = private_key.sign(msg, p, hashes.SHA1())
assertIsNone(public_key.verify(signature, msg, p, hashes.SHA1()))  # 验签成功返回 None

p = PSS(MGF1(hashes.SHA1()), PSS.MAX_LENGTH)
signature = private_key.sign(msg, p, hashes.SHA1())
assertIsNone(public_key.verify(signature, msg, p, hashes.SHA1()))  # 验签成功返回 None
```

## 加密 & 解密

**加密解密过程**

1. B 使用 A 的公钥加密消息 m，得到密文 s
2. B 将密文 s 发送给 A
3. A 使用自己的私钥解密密文 s，得到消息 m

有两种加密解密方案：`PKCS1`、`EME-OAEP`。

**`PKCS1` 方案**：

```python
from cryptography.hazmat.primitives.asymmetric.padding import PKCS1v15

msg = b'hello world'

p = PKCS1v15()
ciphertext = public_key.encrypt(msg, p)
assertEqual(msg, private_key.decrypt(ciphertext, p))
```

**`EME-OAEP` 方案**：

只支持有限的 `hash` 算法：`SHA1`、`SHA224`、`SHA256`、`SHA384`、`SHA512`。

```python
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric.padding import OAEP, MGF1

msg = b'hello world'

p = OAEP(MGF1(hashes.SHA1()), hashes.SHA1(), b'hello')
ciphertext = public_key.encrypt(msg, p)
assertEqual(msg, private_key.decrypt(ciphertext, p))
```
