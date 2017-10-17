# 说说compareTo和compare方法的比较


## 1. compareTo方法介绍

compareTo(Object o)方法是java.lang.**Comparable**<T>接口中的方法，当需要对某个类的对象进行排序时，该类需要实现Comparable<T>接口的，必须重写public int compareTo(T o)方法。

下面说一下，compareTo方法的返回值：

+ 如果指定的数与参数相等返回0
+ 如果指定的数小于参数返回 -1
+ 如果指定的数大于参数返回 1

<br>

比如MapReduce中Map函数和Reduce函数处理的 <key,value>,其中需要根据key对键值对进行排序，所以，key实现了WritableComparable<T>接口，实现这个接口可同时用于序列化和反序列化。WritableComparable<T>接口(用于序列化和反序列化)是Writable接口和Comparable<T>接口的组合；




## 2. compare方法介绍

compare(Object o1,Object o2)方法是java.util.**Comparator**<T>接口的方法，它实际上用的是待比较对象的compareTo(Object o)方法。

通过compare()方法可以用来比较两个基本类型值的大小

同时也可以用来比较两个Boolean类型值

compare()方法  输出结果  大于=1；等于=0；小于=-1; 

## 2. compare方法应用

```java
import org.jetbrains.annotations.NotNull;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class StudentForCompare  {

    private int id;
    private String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public StudentForCompare(int id, String name) {
        this.id = id;
        this.name = name;
    }


    @Override
    public String toString() {
        return "StudentForCompareTo{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }


    public static void main(String[] args) {
        List<StudentForCompare> stus = new ArrayList<>();
        stus.add(new StudentForCompare(11,"AA"));
        stus.add(new StudentForCompare(1,"BB"));
        stus.add(new StudentForCompare(121,"CC"));
        stus.add(new StudentForCompare(341,"DD"));
        stus.add(new StudentForCompare(12,"EE"));
        stus.add(new StudentForCompare(11,"GG"));
        Collections.sort(stus, new Comparator<StudentForCompare>() {
            @Override
            public int compare(StudentForCompare o1, StudentForCompare o2) {

                if (o1.id>o2.id){
                    return 1;
                }else if (o1.id<o2.id){
                    return -1;
                }
                return o1.name.compareTo(o2.name);

            }
        });

        for (StudentForCompare stu:stus) {
            System.out.println(stu);
        }


    }
}

```

输出结果：
```
StudentForCompareTo{id=1, name='BB'}
StudentForCompareTo{id=11, name='AA'}
StudentForCompareTo{id=11, name='GG'}
StudentForCompareTo{id=12, name='EE'}
StudentForCompareTo{id=121, name='CC'}
StudentForCompareTo{id=341, name='DD'}
```



## 2. compareTo方法应用

```java
import org.jetbrains.annotations.NotNull;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class StudentForCompareTo implements Comparable {

    private int id;
    private String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public StudentForCompareTo(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public int compareTo(@NotNull Object o) {
        if (this == o) {
            return 0;
        } else if (o != null && o instanceof StudentForCompareTo) {
            StudentForCompareTo s = (StudentForCompareTo) o;
            if (id < s.id) {
                return -1;
            } else if (id == s.id) {
                return name.compareTo(s.name);
            }

            return 1;

        }
        return -1;
    }

    @Override
    public String toString() {
        return "StudentForCompareTo{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }


    public static void main(String[] args) {
        List<StudentForCompareTo> stus = new ArrayList<>();
        stus.add(new StudentForCompareTo(11,"AA"));
        stus.add(new StudentForCompareTo(1,"BB"));
        stus.add(new StudentForCompareTo(121,"CC"));
        stus.add(new StudentForCompareTo(341,"DD"));
        stus.add(new StudentForCompareTo(12,"EE"));
        stus.add(new StudentForCompareTo(11,"GG"));
        Collections.sort(stus);

        for (StudentForCompareTo stu:stus) {
            System.out.println(stu);
        }


    }
}

```

输出结果：
```
StudentForCompareTo{id=1, name='BB'}
StudentForCompareTo{id=11, name='AA'}
StudentForCompareTo{id=11, name='GG'}
StudentForCompareTo{id=12, name='EE'}
StudentForCompareTo{id=121, name='CC'}
StudentForCompareTo{id=341, name='DD'}
```