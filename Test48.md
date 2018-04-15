# 面试题48：不能被继承的类

题目：用C++设计一个不能被继承的类。

这里用java编写（感觉没啥意义）：

```java

public class Test48 {

    private Test48() {

    }

    //提供外部访问的接口
    public static Test48 getInstance() {
        return new Test48();
    }
}
```
