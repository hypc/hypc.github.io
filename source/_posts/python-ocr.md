---
title: Python图片识别
date: 2019-01-21 13:17:19
tags: [python, ocr, tesseract]
---

使用[Google’s Tesseract-OCR Engine][tesseract]来识别图片。

## 安装

### 安装Tesseract

参考[Install Tesseract via pre-built binary package][tesseract_wiki]安装Tesseract。

### 安装pytesseract

直接使用pip命令安装：

```bash
pip install pytesseract
```

## Quickstart

```python
try:
    from PIL import Image
except ImportError:
    import Image
import pytesseract

# If you don't have tesseract executable in your PATH, include the following:
pytesseract.pytesseract.tesseract_cmd = r'<full_path_to_your_tesseract_executable>'
# Example tesseract_cmd = r'C:\Program Files (x86)\Tesseract-OCR\tesseract'

# Simple image to string
print(pytesseract.image_to_string(Image.open('test.png')))

# French text image to string
print(pytesseract.image_to_string(Image.open('test-european.jpg'), lang='fra'))

# Get bounding box estimates
print(pytesseract.image_to_boxes(Image.open('test.png')))

# Get verbose data including boxes, confidences, line and page numbers
print(pytesseract.image_to_data(Image.open('test.png')))

# Get information about orientation and script detection
print(pytesseract.image_to_osd(Image.open('test.png')))

# In order to bypass the internal image conversions, just use relative or absolute image path
# NOTE: If you don't use supported images, tesseract will return error
print(pytesseract.image_to_string('test.png'))

# get a searchable PDF
pdf = pytesseract.image_to_pdf_or_hocr('test.png', extension='pdf')

# get HOCR output
hocr = pytesseract.image_to_pdf_or_hocr('test.png', extension='hocr')
```

[tesseract]: https://github.com/tesseract-ocr/tesseract
[tesseract_wiki]: https://github.com/tesseract-ocr/tesseract/wiki
