---
title: JUnit多线程参数化测试
date: 2018-09-16 17:42:57
tags: [java]
---

参数化测试本身是不支持多线程测试的，对于一些测试结果没有依赖的测试来说，单线程的测试执行效率相当低下。

通过在测试用例中添加线程租，开启多个线程同时执行测试用例，并且在线程租中线程都执行完的时候结束测试，来达到优化测试效率的目的。

<!--more-->

```java
package org.huyp;

import java.util.Arrays;
import java.util.Collection;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

import org.junit.AfterClass;
import org.junit.Assert;
import org.junit.BeforeClass;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;


@RunWith(Parameterized.class)
public class ThreadParameterizeTest {

    private static ThreadPoolExecutor POOL;
    private final static String[][] params = { { "A", "A" }, { "B", "B" }, { "C", "C" } };
    private String expected;
    private String target;

    @BeforeClass
    public static void beforeClass() {
        System.out.println("---- start test ----");
        POOL = (ThreadPoolExecutor) Executors.newFixedThreadPool(params.length);
    }

    @AfterClass
    public static void afterClass() {
        try {
            while (POOL.getActiveCount() > 0) {
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
        } finally {
            POOL.shutdown();
            System.out.println("---- end test ----");
        }
    }

    public ThreadParameterizeTest(String expected, String target) {
        this.expected = expected;
        this.target = target;
    }

    @Parameters
    public static Collection<String[]> initParams() {
        return Arrays.asList(params);
    }

    @Test
    public void test() {
        POOL.submit(new Runnable() {
            @Override
            public void run() {
                System.out.println(expected + ": " + target);
                Assert.assertEquals(expected, target);
            }
        });
    }
}
```

## 注意

这个类与普通参数化测试类多了几个不同的地方：

1. 在类里面多了一个静态的线程池；

2. 在`@BeforeClass`里面初始化了这个线程池；

3. 在`@AfterClass`里面监听线程池活动的线程数：

    ```java
    while (POOL.getActiveCount() > 0) {
        Thread.sleep(1000);
    }
    ```

    在没有活动的线程时关闭线程池：

    ```java
    POOL.shutdown();
    ```

4. 在`@Test`方法里面创建线程，并用线程池进行管理，而具体的逻辑处理在`run()`方法里面。
