---
title: 代码检查错误列表
date: 2018-08-14 16:17:13
tags: [testing]
---

## 数据引用错误

1. 是否有引用的变量未赋值或未初始化？
2. 下标的值是否在范围之内？
3. 是否存在非整数下标？
4. 是否存在虚调用？
5. 当使用别名时属性是否正确？
6. 记录和结构的属性是否匹配？
7. 是否计算位串的地址？是否传递位串参数？
8. 基础的存储属性是否正确？
9. 跨过程的结构定义是否匹配？
10. 索引或下标操作是否有“仅差一个”的错误？
11. 继承需求是否得到满足？

## 运算错误

1. 是否存在非算术变量间的运算？
2. 是否存在混合模式的运算？
3. 是否存在不同字长变量间的运算？
4. 目标变量的大小是否小于赋值大小？
5. 中间结果是否上溢或下溢？
6. 是否存在被0除？
7. 是否存在二进制的不精确度？
8. 变量的值是否超过了有意义的范围？
9. 操作符的优先顺序是否被正确理解？
10. 整数除法是否正确？

<!--more-->

## 数据声明错误

1. 是否所有的变量都已声明？
2. 默认的属性是否被正确理解？
3. 数组和字符串的初始化是否正确？
4. 变量是否赋予了正确的长度、类型和存储类？
5. 初始化是否与存储类相一致？
6. 是否有相似的变量名？

## 比较错误

1. 是否存在不同类型变量间的比较？
2. 是否存在混合模式的比较运算？
3. 比较运算符是否正确？
4. 布尔表达式是否正确？
5. 比较运算是否与布尔表达式相混合？
6. 是否存在二进制小数的比较？
7. 操作符的优先顺序是否被正确理解？
8. 编译器对布尔表达式的计算方式是否被正确理解？

## 控制流程错误

1. 是否超出了多条分支路径？
2. 是否每个循环都终止了？
3. 是否每个程序都终止了？
4. 是否存在由于入口条件不满足而跳过循环体？
5. 可能的循环越界是否正确？
6. 是否存在“仅差一个”的迭代错误？
7. DO/END语句是否匹配？
8. 是否存在不能穷尽的判断？
9. 输出信息中是否有文字或语法错误？

## 输入/输出错误

1. 文件属性是否正确？
2. OPEN语句是否正确？
3. I/O语句是否符合格式规范？
4. 缓冲大小与记录大小是否匹配？
5. 文件在使用前是否打开？
6. 文件在使用后是否关闭？
7. 文件结束条件是否被正确处理？
8. 是否处理了I/O错误？

## 接口错误

1. 形参的数量是否等于实参的数量？
2. 形参的属性是否与实参的属性相匹配？
3. 形参的量纲是否与实参的量纲相匹配？
4. 传递给被调用模块的实参个数是否等于其形参个数？
5. 传递给被调用模块的实参属性是否与其形参属性匹配？
6. 传递给被调用模块的实参量纲是否与其形参量纲匹配？
7. 调用内部函数的实参的数量、属性、顺序是否正确？
8. 是否引用了与当前入口点无关的形参？
9. 是否改变了某个原本仅为输入值的形参？
10. 全局变量的定义在模块间是否一致？
11. 常数是否以实参形式传递过？

## 其他检查

1. 在交叉引用列表中是否存在未引用过的变量？
2. 属性列表是否与预期的相一致？
3. 是否存在“警告”或“提示”信息？
4. 是否对输入的合法性进行了检查？
5. 是否遗漏了某个功能？
