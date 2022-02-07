---
title: png2icon
date: 2021-07-21 13:44:24
tags:
---

本文的目标便是将图片转成 `.ico` 图标。

**支持图片格式**：

* `png`
* `jpg`
* `jpeg`

**依赖**：

* `sips`：macOS 系统自带的图片处理工具
* `imagemagick`：一套开源的、强大且稳定的图片处理工具包，需提前安装 `brew install imagemagick`

**思路**：

1. 先使用 `sips` 命令得到图片的宽度
2. 然后再使用 `sips` 命令生成 `iconset`
3. 使用 `iconutil` 命令将 `iconset` 转换成 `icns` 文件
4. 最后使用 `convert` 命令将 `icns` 文件转换成 `ico` 文件

<!--more-->

**完整代码如下**：

```bash
function png2icon() {
    # support: png jpg jpeg
    local image_file=$1
    if [[ ! -f "${image_file}" ]]; then
        echo "No such file or directory: ${image_file}"
        exit 1
    fi

    local fmt=$(sips --getProperty format "${image_file}" | grep format | awk '{print $2}')
    local size=$(sips --getProperty pixelWidth "${image_file}" | grep pixelWidth | awk '{print $2}')
    local filename=${image_file%.*}
    local ico_size=32

    mkdir -p "${filename}.iconset/"
    local resizes=(32 64 128 256 512 1024)
    for resize in ${resizes[*]}; do
        if [[ ${size} -ge ${resize} ]]; then
            sips -z ${resize} ${resize} "${image_file}" --out "${filename}.iconset/icon_${resize}x${resize}.${fmt}"
            ico_size=${resize}
        fi
    done

    iconutil -c icns ${filename}.iconset
    if [[ $ico_size -ge 256 ]]; then
        ico_size=256
    fi
    # 需要安装 imagemagick
    convert -resize x${ico_size} -gravity center -crop ${ico_size}x${ico_size}+0+0 "${image_file}" -flatten -colors 256 -background transparent "${filename}.ico"
}
```

**注意**：

* `ico` 文件最大像素为 `256` 像素，建议宽度在 `256` 像素以上的图片生成 `ico` 图标。
* 选择图片时，尽量选择宽高相同的图片，否则会出现 `convert: width or height exceeds limit '***.ico' @ error/icon.c/WriteICONImage/927.` 错误。
