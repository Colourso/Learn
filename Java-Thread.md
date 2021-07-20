## 多线程创建的三种方式

|   创建方式   |   使用场景   |
| :----------: | :----------: |
|  继承Thread  |    单继承    |
| 实现Runnable | 无返回值任务 |
| 实现Callable | 有返回值任务 |

### 1 继承Thread

```java
public class MyThread extends Thread{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " 线程启动运行");
    }

    public static void main(String[] args) {
        new MyThread().start();
    }
}
```

- 由于Java只支持单继承，所以一个类一旦继承了Thread类就不能继承其他类了，对于功能扩展有很大的限制，不推荐使用。

### 2 实现Runnable

```java
public class TestRunnable implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " 线程启动运行");
    }

    public static void main(String[] args) {
        TestRunnable testRunnable = new TestRunnable();
        Thread thread = new Thread(testRunnable);
        thread.start();
    }
}
```

- Runnable仅仅只是一个接口，没有启动线程的能力，需要搭配之Thread类来使用。

#### lambda写法

```java
new Thread(()->{
    System.out.println(Thread.currentThread().getName() + " 线程启动运行");
}).start();
```

- 因为Runnable接口是一个函数式接口，可以简写为lambda表达式。

#### 匿名内部类形式

```java
new Thread(new Runnable(){
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " 线程启动运行");
    }
}).start();
```

### 3 实现Callable接口

```java
public class TestCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        return Thread.currentThread().getName() + " 线程启动运行";
    }

    public static void main(String[] args) {
        TestCallable testCallable = new TestCallable();
        // 创建FutureTask实例
        FutureTask<String> futureTask = new FutureTask<>(testCallable);
        // 创建线程
        Thread thread = new Thread(futureTask);
        // 启动线程
        thread.start();
        try {
            // 获取任务执行的结果
            String result = futureTask.get();
            System.out.println(result);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

- 需要指明类型，即继承时的泛型类型要指明。
- Callable仅仅只是一个接口，没有启动线程的能力，需要搭配之Thread类来使用。