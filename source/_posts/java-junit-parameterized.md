---
title: JUnit参数化测试
date: 2018-09-16 17:32:23
tags: [java]
---

参数化测试的编写稍微有点麻烦（当然这是相对于`JUnit`中其它特性而言）：

1. 为准备使用参数化测试的测试类指定特殊的运行器`org.junit.runners.Parameterized`。
2. 为测试类声明几个变量，分别用于存放期望值和测试所用数据。
3. 为测试类声明一个使用注解`org.junit.runners.Parameterized.Parameters`修饰的，
    返回值为`java.util.Collection`的公共静态方法，并在此方法中初始化所有需要测试的参数对。
4. 为测试类声明一个带有参数的公共构造函数，并在其中为第二个环节中声明的几个变量赋值。
5. 编写测试方法，使用定义的变量作为参数进行测试。

<!--more-->

```java
package hypc;

import org.junit.Assert;
import java.util.Arrays;
import java.util.Collection;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;


@RunWith(Parameterized.class)
public class ParameterizeTest {

    private String expected;
    private String target;

    @Parameters
    public static Collection words() {
        return Arrays.asList(new Object[][] {
            { "employee_info", "employeeInfo" },    // 测试一般的处理情况
            { null, null },                         // 测试 null 时的处理情况
            { "", "" },                             // 测试空字符串时的处理情况
            { "employee_info", "EmployeeInfo" },    // 测试当首字母大写时的情况
            { "employee_info_a", "employeeInfoA" }, // 测试当尾字母为大写时的情况
            { "employee_a_info", "employeeAInfo" }  // 测试多个相连字母大写时的情况
        });
    }

    /**
     * 参数化测试必须的构造函数
     *
     * @param expected
     *            期望的测试结果，对应参数集中的第一个参数
     * @param target
     *            测试数据，对应参数集中的第二个参数
     */
    public ParameterizeTest(String expected, String target) {
        this.expected = expected;
        this.target = target;
    }

    @Test
    public void wordFormat() {
        Assert.assertEquals(expected, WordDealUtil.wordFormat(target));
    }
}
```

**注意**：运行时是运行类，即`ParameterizeTest`，而不是方法`wordFormat`。
