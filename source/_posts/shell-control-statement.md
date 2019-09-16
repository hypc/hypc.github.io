---
title: Shell控制语句
date: 2019-09-16 08:44:17
tags: [shell]
---

## If语句

if语法格式如下：

```bash
if condition1; then     # if语句块，必须出现一次
    ...
elif condition2; then   # elif语句块，可出现0次或多次
    ...
else                    # else语句块，只能出现0次或1次
    ...
fi
```

示例，判断两个数是否相等：

```bash
a=10
b=20
if [ $a == $b ]; then
   echo "a 等于 b"
elif [ $a -gt $b ]; then
   echo "a 大于 b"
elif [ $a -lt $b ]; then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

<!--more-->

## for语句

for语句一般格式为：

```bash
for var in item1 item2 ... itemN; do
    ...
done
```

可以通过一些技巧快速生成序列：

```bash
for f in $(ls); do ... done             # 遍历当前文件夹下所有文件（夹）
for i in {1..10}; do ... done           # 遍历1~10
for c in {a..f}; do ... done            # 遍历a~f
for i in $(seq 1 10); do ... done       # 遍历1~10
for i in $(seq 1 2 10); do ... done     # 遍历1~10，但步长为2
```

## while语句

while语句一般格式为：

```bash
while condition; do
    ...
done
```

示例如下：

```bash
int=1
while [ $int -lt 5 ]; do
    echo $int
    let "int++"
done
```

## until语句

同while语句一样，但与while语句不同的是，while语句中条件为真时，才执行循环语句块，
而until语句中，条件为假时才执行循环语句块，这一点恰好相反。

```bash
until condition; do
    ...
done
```

示例如下：

```bash
int=10
until [ $int -lt 5 ]; do
    echo $int
    let "int--"
done
```

## case语句

case语句块与其他语言中的switch非常相像，语法格式如下：

```bash
case var in
mode1)          # 匹配模式1
    ...
    ;;
mode2|mode3)    # 当多个模式使用同一段语句块时，可使用`|`进行分隔
    ...
    ;;
*)              # 其他模式匹配不了时，执行该语句块，可选
    ...
    ;;
esac
```

示例代码如下：

```bash
read -p "请输入选项（1, 2, 3）: " choice
case $choice in
1)
    echo "你选择了1"
    ;;
2)
    echo "你选择了2"
    ;;
3)
    echo "你选择了3"
    ;;
*)
    echo "你只能在1、2、3中进行选择"
    ;;
esac
```

## break和continue

continue的用法与其他语言的用法基本一致，都是用来终止本次循环并进行下一次循环。

break表示跳出循环，但与其他语言不同的是，如果是多重嵌套循环，
break表示跳出所有循环，而不是跳出一层循环，如果仅想跳出一层或n层循环，可在后面添加一个数字用来表示跳出循环的层数。

```bash
read -p "请输入选项（1, 2, 3）: " choice
while true; do
    case $choice in
    1|2|3)
        echo "你的选项为: $choice"
        break
        ;;
    *)
        read -p "你的输入不符合要求，请重新输入: " choice
        continue
        ;;
    esac
done
```
