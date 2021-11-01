# ThreadLocal 使用空间，规避线程安全问题
```java
package com.spring.commonUtils;

public class ThreadLocalTest {

    /*
    使用空间，规避线程安全问题
     */
    private static ThreadLocal<String> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                //存入线程名称
                threadLocal.set(Thread.currentThread().getName());
                int i = 0;
                while (i < 10) {
                    try {
                        Thread.currentThread().sleep(1000);
                        /**
                         * 线程001存入的信息，只有线程001可以获取
                         * 线程002存入的信息，只有线程002可以获取
                         */
                        String str = threadLocal.get();
                        System.err.println(str + "---" + i);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    i++;
                }
            }
        };


        Thread thread001 = new Thread(runnable);
        thread001.setName("001-");
        thread001.start();
        Thread thread002 = new Thread(runnable);
        thread002.setName("   002-");
        thread002.start();


    }


}
```
