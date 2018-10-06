---
title: Moodle：使用GIFT格式导入试题
date: 2018-10-06 13:54:45
tags: [moodle]
---

GIFT格式是一种编写可导入试题的格式，它可用来编写选择题、判断题、简答题、填空题以及数字题。
GIFT格式可是题库可导出格式的一种。这种格式是[Moodle][]社区内开发的，其他软件或多或少支持它。

## 格式符号

符号                         |说明
----------------------------|-------------------------------------------------------------------------------------
`// text`                   |Comment until end of line (optional)
`::title::`                 |Question title (optional)
`text`                      |Question text (becomes title if no title specified)
`[...format...]`            |The format of the following bit of text. Options are `[html]`, `[moodle]`, `[plain]` and `[markdown]`. The default is `[moodle]` for the question text, other parts of the question default to the format used for the question text.
`{`                         |Start answer(s) -- without any answers, text is a description of following questions
`{T}` or `{F}`              |True or False answer; also `{TRUE}` and `{FALSE}`
`{ ... =right ... }`        |Correct answer for multiple choice, (multiple answer? -- see page comments) or fill-in-the-blank
`{ ... ~wrong ... }`        |Incorrect answer for multiple choice or multiple answer
`{ ... =item -> match ... }`|Answer for matching questions
`#feedback text`            |Answer feedback for preceding multiple, fill-in-the-blank, or numeric answers
`####general feedback`      |General feedback
<code>&#123;&#35;</code>    |Start numeric answer(s)
`answer:tolerance`          |Numeric answer accepted within ± tolerance range
`low..high`                 |Lower and upper range values of accepted numeric answer
`=%n%answer:tolerance`      |n percent credit for one of multiple numeric ranges within tolerance from answer
`}`                         |End answer(s)
`\character`                |Backslash escapes the special meaning of `~`, `=`, `#`, `{`, `}`, and `:`
`\n`                        |Places a newline in question text -- blank lines delimit questions

<!--more-->

## 问题格式示例

### 选择题

选择题分为单选题和多选题。

#### 单选题

简单的格式如下：

```text
Who's buried in Grant's tomb? {=Grant ~no one ~Napoleon ~Churchill ~Mother Teresa }

Moodle costs {~lots of money =nothing ~a small amount} to download from moodle.org.

// question: 1 name: Grants tomb
::Grants tomb::Who is buried in Grant's tomb in New York City? {
    =Grant
    ~No one
    #Was true for 12 years, but Grant's remains were buried in the tomb in 1897
    ~Napoleon
    #He was buried in France
    ~Churchill
    #He was buried in England
    ~Mother Teresa
    #She was buried in India
}

Mahatma Gandhi's birthday is an Indian holiday on {
    ~15th
    ~3rd
    =2nd
} of October.

Since {
    ~495 AD
    =1066 AD
    ~1215 AD
    ~ 43 AD
} the town of Hastings England has been "famous with visitors".
```

#### 多选题

```text
What two people are entombed in Grant's tomb? {
    ~%-100%No one
    ~%50%Grant
    ~%50%Grant's wife
    ~%-100%Grant's father
}
```

### 判断题

```text
// question: 0 name: TrueStatement using {T} style
::TrueStatement about Grant::Grant was buried in a tomb in New York City.{T}

// question: 0 name: FalseStatement using {FALSE} style
::FalseStatement about sun::The sun rises in the West.{FALSE}
```

### 匹配题

```text
Match the following countries with their corresponding capitals. {
    =Canada -> Ottawa
    =Italy  -> Rome
    =Japan  -> Tokyo
    =India  -> New Delhi
}
```

### 填空题

填空题答案都是以`=`号开头，不能使用`~`。

```text
Who's buried in Grant's tomb? {=Grant =Ulysses S. Grant =Ulysses Grant}

Two plus two equals {=four =4}

Moodle costs {=nothing} to download from moodle.org.
```

如果只有一个正确的答案，可以不写`=`，只要它不被误识别为判断题即可。

### 数字题

```text
// It will accept a range of 5 years.
When was Ulysses S. Grant born?{#1822:5}

What is the value of pi (to 3 decimal places)? {#3.141..3.142}.

When was Ulysses S. Grant born? {#
    =1822:0
    =%50%1822:2
}
```

### 简答题

```text
Write a short biography of Dag Hammarskjöld. {}
```

[Moodle]: https://moodle.org/
