[TOC]

***
# JAVA SE知识图谱
* java 发展发程
* java 环境搭建
* 基础程序设计
    * 数据类型
    * 运算符
    * 流程控制
    * 数组
* 面向对象编程
    * 类和对象
    * 属性
    * 方法
    * 三大特性
    * 接口
    * 设计模式
* java 新特性
    * 泛型
    * 枚举
    * 装箱/拆箱
    * 可变参数 
    * Annotation
* 应用程序开发
    * JDBC
    * 集合
    * 异常处理
    * 类库
    * 多线程
    * IO
    * 反射
    * 网络

***

# JAVA 学习笔记 



## JAVA 多线程

### 基本概念

1. 概念 模型
 	1. 进程
 	1. 线程
 	1. 程序

 1. 创建多线程
 	1. Thread

 	1. Runnable

 	重写 run()方法

 1. 启动 
 start()

 1. 线程的生命周期
 	* 新建
 	* 可执行
 	* 等待 wait() 之后让别人执行 之后再自己竞争
 	* 执行 run()
 	* 阻塞
 	* 死亡

 	```sequence
 	新建->可执行:start()
 	可执行->执行:
 	执行->可执行: yield()
 	执行->阻塞: sleep() join() IO 被锁资源
 	阻塞->可执行: interrupt() sleep超时 join执行完成 IO完成
 	执行->等待: wait()
 	等待->执行:
 	执行->死亡:

 	```

 1. 线程 同步 通信
 	* synchronize 关键字
 	* wait()
 	* notify() notifyAll()
 	两者区别只是在于一个是通知单个，一个是通知所有的等待


### 线程实例

> 在测试程序时 不要用 junit 测试多线程
原因：junit 是起了单独一个线程来跑测试方法 如果方法运行完了，就直接 调用 System.exit(0) 结束 JVM 后台运行的程序 就会直接挂掉
措施：要么打一个补充 jar --> groboutils 但这个 mvnresposity 里没有
要么就直接起一个 main 方法，在 main 方法里面测试

1. 创建和启动
	* Thread

    > 创建 

    ```
    public class Demo1Thread extends Thread {

        @Override
        public void run() {

	        for (int i = 0; i < 1000; ++i) {
	            System.out.println("当前线程 :" + getName() + "  正在打印 :" + i);
	        }
        }
    }
    ```
    > 启动


    ```
    @Test
    public void demo1() {
        Demo1Thread d1t = new Demo1Thread();
        Demo1Thread d2t = new Demo1Thread();
        d1t.start();
        d2t.start();
    }

    ```

    * Runnable

    > 创建

    ```
    public class Demo2Runnable implements Runnable {
        @Override
        public void run() {
            String name = Thread.currentThread().getName();
            for (int i = 0; i < 1000; i++) {
                System.out.println("current thread : " + name + "   |||| printing : " + i);
                if (i % 10 == 0) {
                    Thread.currentThread().yield();
                    System.out.println();
                }
            }
        }
    }
    ```

      > 启动

    ```
    @Test
    public void demo2() {
        Thread td1 = new Thread(new Demo2Runnable());
        Thread td2 = new Thread(new Demo2Runnable());

        td1.start();
        td2.start();

    }

    ```

1. 同步

```
<!-- 并发类 -->
public class Demo3bingfa implements Runnable {

    private int appleNum = 10000;
    private boolean isRun = true;

    @Override
    public void run() {

        while (isRun) {
            isRun = getApple();
        }
        if (appleNum <= 0) {
            System.out.println(Thread.currentThread().getName() + " thread is dead !!!");
            return;
        }
    }

    private synchronized boolean getApple() {
        if (appleNum == 0)
            return false;

        appleNum--;

        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        if (appleNum >= 0) {
            System.out.println(Thread.currentThread().getName() + " get one apple , the num of apples is :" + appleNum);
        } else {
            System.out.println("the apple is all gone !!!");
        }

        return true;

    }
}

<!-- 测试程序  -->
    @Test
    public void demo3() {
        Demo3bingfa bf = new Demo3bingfa();
        Thread td1 = new Thread(bf);
        Thread td2 = new Thread(bf);
        td1.setName("ming");
        td2.setName("qiang");

        td1.start();
        td2.start();

    }


```

1. 通信

```java
<!-- 多线程通信类 -->
<!-- 卖票的程序 5元一张票 如果没钱找就 等待，下一位买 -->

<!-- synchronize wait() notify() notifyAll() -->

public class Demo4co implements Runnable {

    private int fiveAmount = 1;
    private int tenAmount = 0;
    private int twentyAmount = 0;


    @Override
    public void run() {
        if (Thread.currentThread().getName().equals("1")) {
            saleTicket(20);
        } else if (Thread.currentThread().getName().equals("2")) {
            saleTicket(5);
        } else if (Thread.currentThread().getName().equals("3")) {
            saleTicket(5);
        } else if (Thread.currentThread().getName().equals("4")) {
            saleTicket(5);
        } else if (Thread.currentThread().getName().equals("5")) {
            saleTicket(20);
        } else if (Thread.currentThread().getName().equals("6")) {
            saleTicket(5);
        }else if (Thread.currentThread().getName().equals("7")) {
            saleTicket(5);
        }
    }

    private synchronized void saleTicket(int money) {
        if (money == 5) {
            fiveAmount++;
            System.out.println(Thread.currentThread().getName() + " `s money : " + money + " is well ,here is " + Thread.currentThread().getName() + "`s tichet!");
        } else if (money == 20) {
            while (fiveAmount < 3) {
                <!-- 用 while 如果下次执行还是不满足条件，接着 wait() -->
                try {
                    System.out.println(Thread.currentThread().getName() + " 找不了钱 " + " 下一位 ");
                    wait();
                    Thread.sleep(1000);
                    System.out.println(Thread.currentThread().getName() + " 继续买票");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            fiveAmount -= 3;
            twentyAmount++;
            System.out.println(Thread.currentThread().getName() + "`s money : 20 , here is ticket ");
        }
        notify();


    }
}
 <!-- 测试程序  -->

    @Test
    public void demo4() {
        Demo4co co = new Demo4co();
        Thread td1 = new Thread(co);
        Thread td2 = new Thread(co);
        Thread td3 = new Thread(co);
        Thread td4 = new Thread(co);
        Thread td5 = new Thread(co);
        Thread td6 = new Thread(co);
        Thread td7 = new Thread(co);

        td1.setName("1");
        td2.setName("2");
        td3.setName("3");
        td4.setName("4");
        td5.setName("5");
        td6.setName("6");
        td7.setName("7");

        td1.start();
        td2.start();
        td3.start();
        td4.start();
        td5.start();
        td6.start();
        td7.start();
    }
```

## JAVA IO 处理

### 基本概念
#### 基本模型
1. 模型图
```sequence
文件->程序:输  入
程序->文件:输  出
```
2. 基本要素
    1. 数据
   > 数据分为两种类型:字节与字符

        1. 字节: byte[] 主要用来传输图像文件 压缩文件类型
        1. 字符: char[] 也可以用 String 来接收 主要用来传输文本文件,比如 http 文本响应等等
    1. I O
   > 代表数据流动的方向

        1. I : InputStream -- 字节型   Reader -- 字符型
        1. O : OutputStream -- 字节型   Writer -- 字符型
    1. 流 : 数据的流动过程
        * 节点流 : 文件与流的连接部分
        * 处理流 : 处理流,给流以各种扩展功能
    1. 源/目的 文件类型 : file Object 数组 管道 推回 特殊 打印 String 过滤

### 流的设计

1. 主要是用装饰者模式设计流 主类是四个类，两种数据类型的 in out.

1. 如果有字符转字节的过程，就要考虑编码的问题。否则不需要考虑编码问题

1. 理解 IO 的流程与常用的方法，IO 其实很简单


    
### 怎么用

```
File file  = new File("");

InputStream is = new FileInputStream(file);

int n ;

StringBuffer sb = new StringBuffer();

while((n = is.read()) != -1 ){
    sb.append((char)n);
}

is.close();

OutputStream os = new FileOutputStream(file);

String str = "king of world";

os.write(str.getBytes());

os.flush();

os.write();

```

#### 构造与方法


#### 实例

* 实例如下 注解说明

```
    <!-- 将字符串写入一个文件，默认编码为 UTF-8 -->
    @Test
    public void demo1() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile");
            FileOutputStream fo = new FileOutputStream(file);
            String str = "hello world";
            fo.write(str.getBytes());
            fo.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 同上 -->
    @Test
    public void demo2() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile");
            FileOutputStream fo = new FileOutputStream(file);
            String str = "走进新世界 刘纯彰";
            //fo.write(str.getBytes("utf8"));
            fo.write(str.getBytes());
            fo.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 读取文件内容 由于默认编码是 UTF-8 不存在乱码的问题 -->
    @Test
    public void demo3() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile");
            FileInputStream fi = new FileInputStream(file);
            int n = fi.read();
            do {
                char cr = (char) n;
                n = fi.read();
                System.out.print(cr);
            } while (n != -1);

            fi.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 写入文件，不过有一个字符集的问题，通过 Writer 类来写入文本 -->
    @Test
    public void demo4() {

        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile1");
            FileOutputStream fo = new FileOutputStream(file);
            //OutputStreamWriter osw = new OutputStreamWriter(fo, "utf8");
            OutputStreamWriter osw = new OutputStreamWriter(fo, "gbk");
            String str = "走进新世界 我是世界之王";
            osw.write(str);
            osw.flush();
            osw.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 通过 Reader 来读取文件内容 -->
    @Test
    public void demo5() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile1");
            FileInputStream fi = new FileInputStream(file);
            //InputStreamReader isr = new InputStreamReader(fi,"utf8");
            InputStreamReader isr = new InputStreamReader(fi, "gbk");
            StringBuffer sb = new StringBuffer();
            while (isr.ready()) {
                sb.append((char) isr.read());
            }

            System.out.println(sb.toString());

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 直接进行文本操作 -->
    @Test
    public void demo6() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile2");
            FileWriter fw = new FileWriter(file, true);
            String str = "this is the line ";
            for (int i = 0; i < 100; i++) {
                fw.write(str + i + "\n");
            }
            fw.flush();
            fw.close();
            FileReader fr = new FileReader(file);
            StringBuffer sb = new StringBuffer();
            while (fr.ready()) {
                sb.append((char) fr.read());
            }
            System.out.println(sb.toString());
            fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    <!-- 自带 buffer 提高性能 -->
    @Test
    public void demo7() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/newfile3");
            BufferedWriter bw = new BufferedWriter(new FileWriter(file, true));
            String str = "这是第";
            for (int i = 0; i < 100; i++) {
                bw.write(str + i + "行 ! \n");
            }
            bw.flush();
            bw.close();
            BufferedReader br = new BufferedReader(new FileReader(file));
            while (br.ready()) {
                System.out.println(br.readLine());
            }

            br.close();

        } catch (IOException e) {
            e.printStackTrace();
        }

    }


    <!-- 将一个文件的内容复制到另一个文件 -->
    @Test
    public void demo8() {
        try {
            File file = new File("/Users/liuchunzhang/Desktop/filefrom");
            FileWriter fw = new FileWriter(file, true);
            String str = "this is the content of the file  ";
            for (int i = 0; i < 10; i++) {
                fw.write(str + i + "\n");
            }
            fw.close();
            FileInputStream fi = new FileInputStream(file);
            byte bts[] = new byte[1024];
            File file1 = new File("/Users/liuchunzhang/Desktop/fileto");
            FileOutputStream fo = new FileOutputStream(file1);
            int len;
            while ((len = fi.read(bts)) > 0) {
                fo.write(bts, 0, len);
            }
            fo.flush();
            fo.close();
            fi.close();
            FileReader fr = new FileReader(file1);
            while (fr.ready()) {
                System.out.print((char) fr.read());
            }
            fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```

----------------

## JAVA 反射

### 基本概念

### 反射实例

#### 动态代理
1. 动态代理主要用于面向切面编程
1. 由于代理的原理实现是通过 类型的强制转换实现的，所以必须要有公共接口
1. 代理对象为接口声明
1. 做日志，验证，统计，权限

代码实例

```
<!-- 代理类 -->
public Class LoggingHandler implements InvocationHandler{
    
    private Object target;

    public LoggingHandler (Object target){
        this.target = target;
    }

    @Overright

    public Object invoke(Object object,Method method,Object[] args){
        <!-- 前面的处理逻辑 -->
         Object result = method.invoke(target,args);
         <!-- 后面的处理逻辑 -->

    }

    public Object createProxy(Object target){
        return new Proxy.newProxyInstance(target.getClass().getClassLoader,target.getClass().getInterfaces(),new LoggingHandler(target));
    }


}

<!-- 实体类 -->

public interface Caculator{
    
    int add(int a);
}

public class CaculatorImp{
    
    public int add(int a){
        <!-- 业务处理 -->
        return a+100;
    }
}

<!-- 使用动态代理实现面向切面编程 -->

@Test
public void demoTest(){
    Caculator caculatorImp = new CaculatorImp();

    <!-- 生成动态代理对象 -->

    Caculator caculator = (Caculator)LoggingHandler.createProxy(caculatorImp);

    caculator.add(10);

}

```

```
<!-- 不写对应的代理类，一句话搞定 -->
public interface Caculator{
    
    double add(double a,double b);
}

public class CaculatorImp{
    
    public double div(double a,double b){
        <!-- 业务处理 -->
        return a+100;
    }
}

@Test
public void demo2(){

    <!-- 注意 加上 final 关键字 -->
    final Caculator caculator = new CaculatorImp();

    Caculator caculator1 = (Caculator) Proxy.newProxyInstance(caculator.getClass().getClassLoader(), caculator.getClass().getInterfaces(), new InvocationHandler() {
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            System.out.println("begin");
            Object result = method.invoke(caculator,args);
            System.out.println("after");
            return result;
        }
    });
    caculator1.div(1,2);
}
```














