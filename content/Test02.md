## 面试题２：实现Singleton模式


题目：设计一个类，我们只能生成该类的一个实例

```java

public class Test02 {


    /**
     * 　饿汉试，线程非安全
     */
    public static class Singleton1 {
        private final static Singleton1 INSTANCE = new Singleton1();

        private Singleton1() {

        }

        public static Singleton1 getInstance() {
            return INSTANCE;
        }
    }

    /**
     * 懒汉式，线程不安全
     */
    public static class Singleton2 {
        private static Singleton2 instance = null;

        private Singleton2() {

        }

        public static Singleton2 getInstance() {
            if (instance == null) {
                instance = new Singleton2();
            }
            return instance;
        }

    }

    /**
     * 懒汉式，线程安全，多线程环境下效率不高
     */
    public static class Singleton3 {
        private static Singleton3 instance = null;

        private Singleton3() {

        }

        public synchronized static Singleton3 getInstance() {
            if (instance == null) {
                instance = new Singleton3();
            }

            return instance;
        }
    }


    /**
     * 懒汉式，变种，线程安全
     */
    public static class Singleton4 {
        private static Singleton4 instance = null;

        static {
            instance = new Singleton4();
        }

        public static Singleton4 getInstance() {
            return instance;
        }
    }


    /**
     * 使用静态内部类，线程安全【推荐】
     */

    public static class Singleton5 {
        private final static class SingletonHolder {
            private final static Singleton5 INSTANCE = new Singleton5();
        }

        private Singleton5() {

        }

        private static Singleton5 getInstance() {
            return SingletonHolder.INSTANCE;
        }

    }

    /**
     * 使用枚举方式，线程安全[推荐]
     */

    public static enum Singleton6 {
        INSTANCE;

        private Singleton6() {

        }

        public static Singleton6 getInstance() {
            return INSTANCE;
        }
    }


    /**
     * 使用双重校验锁，线程安全【推荐】
     */
    public static class Singleton7 {
        private Singleton7() {

        }

        private static volatile Singleton7 instance = null;

        public static Singleton7 getInstance() {
            if (instance == null) {
                synchronized (Singleton7.class) {
                    if (instance == null) {
                        instance = new Singleton7();
                    }
                }
            }
            return instance;
        }
    }


    public static void main(String[] args) {
        System.out.println(Singleton1.getInstance() == Singleton1.getInstance());
        System.out.println(Singleton2.getInstance() == Singleton2.getInstance());
        System.out.println(Singleton3.getInstance() == Singleton3.getInstance());
        System.out.println(Singleton4.getInstance() == Singleton4.getInstance());
        System.out.println(Singleton5.getInstance() == Singleton5.getInstance());
        System.out.println(Singleton6.INSTANCE == Singleton6.INSTANCE);
        System.out.println(Singleton7.getInstance() == Singleton7.getInstance());
    }
}
```







