# 断言开启的测试

在实验二的测试代码框架中发现

```
@Test(expected=AssertionError.class)
public void testAssertionsEnabled() {...}
```

笔者误以为这个是普通的测试方法，测试后发现在`testAssertionsEnabled()`中本应该测试通过的内容未通过，报错内容是

```bash
java.lang.AssertionError: Expected exception: java.lang.AssertionError
```

甚至在测试中通过控制台输出要比较的内容进行比较，确认比较内容是通过的

查询资料后发现，该方法是用于测试EA断言是否开启的，这个方法期待抛出`AssertionError`类型的错误，也就是说，方法体要抛出任意一个`AssertionError`类型的错误才能通过此测试。

```java
@Test(expected=AssertionError.class)
public void testAssertionsEnabled() {
    assert(false);
}
```

如果EA断言是开启的，`assert(false)`抛出`AssertionError`类型的错误，`expected=AssertionError.class`满足；反之不会抛出，`expected=AssertionError.class`不满足。故此方法可以判断A断言是否开启。

​	