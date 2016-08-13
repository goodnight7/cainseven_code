[TOC]

## 配置 两种方式

### spring mvc 配置文件 配置线程池的属性 
* #### xml
```
<bean id="threadPoolTaskExecutor"
     class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
   <property name="corePoolSize" value="10" />
   <property name="maxPoolSize" value="300" />
   <property name ="keepAliveSeconds" value ="30000" />
   <property name="queueCapacity" value="200" />
</bean>
```


> 当一个任务通过execute(Runnable)方法欲添加到线程池时：
1、 如果此时线程池中的数量小于corePoolSize，即使线程池中的线程都处于空闲状态，也要创建新的线程来处理被添加的任务。
2、 如果此时线程池中的数量等于 corePoolSize，但是缓冲队列 workQueue未满，那么任务被放入缓冲队列。
3、如果此时线程池中的数量大于corePoolSize，缓冲队列workQueue满，并且线程池中的数量小于maximumPoolSize，建新的线程来处理被添加的任务。
4、 如果此时线程池中的数量大于corePoolSize，缓冲队列workQueue满，并且线程池中的数量等于maximumPoolSize，那么通过 handler所指定的策略来处理此任务。也就是：处理任务的优先级为：核心线程corePoolSize、任务队列workQueue、最大线程 maximumPoolSize，如果三者都满了，使用handler处理被拒绝的任务。
5、 当线程池中的线程数量大于 corePoolSize时，如果某线程空闲时间超过keepAliveTime，线程将被终止。这样，线程池可以动态的调整池中的线程数。

   * #### 自动装配
```
@Autowired
ThreadPoolTaskExecutor threadPoolTaskExecutor;
```
### 直接初始化
```
ThreadPoolTaskExecutor poolTaskExecutor = new ThreadPoolTaskExecutor();  
//线程池所使用的缓冲队列  
poolTaskExecutor.setQueueCapacity(200);  
//线程池维护线程的最少数量  
poolTaskExecutor.setCorePoolSize(5);  
//线程池维护线程的最大数量  
poolTaskExecutor.setMaxPoolSize(1000);  
//线程池维护线程所允许的空闲时间  
poolTaskExecutor.setKeepAliveSeconds(30000);  
poolTaskExecutor.initialize();  
```
## 使用

```
threadPoolTaskExecutor.execute(new Runnable() {
    @Override
    public void run() {
       // doSomething;
    }
});
```
## 查看线程池的状态
        可以看方法,自行调用
```
int count = threadPoolTaskExecutor.getActiveCount();
```
