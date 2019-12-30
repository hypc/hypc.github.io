---
title: Shell数组
date: 2019-12-10 09:44:09
tags: [shell]
---

## 数组

**定义数组**

数组使用小括号括起，元素之间使用空格分隔：

```bash
$ arr=(1 2 3 4 5 6)
```

**获取数组长度**

```bash
$ echo ${#arr[*]}
6
$ echo ${#arr[@]}
6
```

**读取元素**

数组下标从`0`开始，若数组对应下标无元素，输出位空。下标使用`*`或`@`输出整个数组：

```bash
$ echo ${arr[0]}
1
$ echo ${arr[1]}
2
$ echo ${arr[2]}
3
$ echo ${arr[10]}

$ echo ${arr[*]}
1 2 3 4 5 6
$ echo ${arr[@]}
1 2 3 4 5 6
```

<!--more-->

**对元素赋值**

直接使用`数组名[下标]`赋值，数组是自动伸缩的，所以不会有数组下标越界错误，
当赋值下标超过元素长度时，中间填充的元素为空：

```bash
$ arr[2]=5
$ arr[8]=12
$ echo ${arr[*]}
1 5 3 4 5 6 12
$ echo ${#arr[*]}
8
$ echo ${arr[7]}

$ echo ${arr[8]}
12
```

**删除元素**

使用`unset 数组名[下标]`删除元素，使用`unset 数组名`删除整个数组：

```bash
$ unset arr[4]
$ echo ${arr[*]}
1 5 3 4 6 12
$ unset arr
$ echo ${arr[*]}

$
```

**数组切片**

使用`${数组名[@|*]:起始下标:长度}`进行切片，长度不写表示一直到数组结尾：

```bash
$ arr=(1 2 3 4 5 6 7 8 9)
$ arr2=( ${arr[*]:5:3} )
$ echo ${arr2[*]}
6 7 8
$ arr3=( ${arr[*]:3} )
$ echo ${arr3[*]}
4 5 6 7 8 9
```

## 数组遍历

**for循环遍历**

```bash
arr=(1 2 3 4 5)
for ele in ${arr[*]}; do
    echo ${arr}
done
```

**按下标遍历数组**

```bash
arr=(1 2 3 4 5)
for i in ${!arr[*]}; do
    echo "index: $i, element: ${arr[$i]}"
done
```

**while循环遍历**

```bash
arr=(1 2 3 4 5)
i=0
while [ $i -lt ${#arr[*]} ]; do
    echo "index: $i, element: ${arr[$i]}"
    let i++
done
```
