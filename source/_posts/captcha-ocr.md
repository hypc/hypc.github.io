---
title: 验证码识别
date: 2019-01-29 08:57:19
tags: [tesseract]
---

使用[Google’s Tesseract-OCR Engine][tesseract]来做验证码识别。

## 安装Tesseract

参考[Install Tesseract via pre-built binary package][tesseract_wiki]安装Tesseract。

### MacOS

在macox上安装时，使用`brew install --with-training-tools tesseract`命令安装。

## 识别验证码

识别验证码可以按照以下步骤进行：

1. 处理验证码图片
   1. 去掉背景，有些验证码背景是纯色背景，这样的验证码我们可以先去掉背景色
   2. 去噪/干扰线，去掉噪点以及干扰线，有些干扰线是1像素宽度，这样可以使用去噪点的方式去除干扰线
   3. 二值化，将图片做二值化处理
   4. 图片切割，将图片切分成单个字符的小图片，也可不切割
2. 识别验证码

<!--more-->

## 字体库训练

为了提高验证码的识别率，我们需要先对[Tesseract][]进行训练，生成自己的字体库。

这里我们使用[jTessBoxEditor][]工具。

先设置一些环境变量：

### 0. 准备验证码图片

准备一些处理后的验证码图片，最好100张以上。

### 1. 合并图片

打开[jTessBoxEditor][]，使用`Tools -> Merge TIFF`合并验证码图片，
将合并后的图片命名为`eng.captcha.exp0.tif`。

### 2. 生成box文件

```bash
tesseract eng.captcha.exp0.tif eng.captcha.exp0 -l captcha --psm 7 batch.nochop makebox
```

### 3. 修改box文件

打开[jTessBoxEditor][]，编辑box文件内容：

![](/images/jTessBoxEditor.png)

### 4. 生成font_properties

```bash
echo captcha 0 0 0 0 0 > font_properties
```

### 5. 生成训练文件

```bash
tesseract eng.captcha.exp0.tif eng.captcha.exp0 -l captcha --psm 7 nobatch box.train
```

### 6. 生成字符集文件

```bash
unicharset_extractor eng.captcha.exp0.box
```

### 7. 生成shape文件

```bash
shapeclustering -F font_properties -U unicharset -O eng.unicharset eng.captcha.exp0.tr
```

### 8. 生成聚集字符特征文件

```bash
mftraining -F font_properties -U unicharset -O eng.unicharset eng.captcha.exp0.tr
```

### 9. 生成字符正常化特征文件

```bash
cntraining eng.captcha.exp0.tr
```

### 10. 更名

```bash
rename normproto captcha.normproto
rename inttemp captcha.inttemp
rename pffmtable captcha.pffmtable
rename unicharset captcha.unicharset
rename shapetable captcha.shapetable
```

### 11. 合并训练文件，生成captcha.traineddata

```bash
combine_tessdata captcha.
```

[tesseract]: https://github.com/tesseract-ocr/tesseract
[tesseract_wiki]: https://github.com/tesseract-ocr/tesseract/wiki
[jTessBoxEditor]: http://vietocr.sourceforge.net/training.html
