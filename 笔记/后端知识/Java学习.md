# 集合排序

## 1.自然排序：

参与排序的对象需实现comparable接口,重写其compareTo()方法,方法体中实现对象的比较大小规则

```java
@Override
  public int compareTo(Object o) {
          if(o instanceof Emp){
              Emp emp = (Emp) o;
              return this.age-emp.getAge();//按照年龄升序排序
             //return this.name.compareTo(emp.getName());//换姓名升序排序
          }
          throw new ClassCastException("不能转换为Emp类型的对象...");
}
```

 

## 2.定制排序,或自定义排序

需编写匿名内部类,先new一个Comparator接口的比较器对象c,同时实现compare()其方法; 

然后将比较器对象c传给Collections.sort()方法的参数列表中,实现排序功能;

```java
Collections.sort(list,new Comparator () {
             @Override
              public int compare(Object o1, Object o2) {
                  if(o1 instanceof Emp && o2 instanceof Emp){
                     Emp e1 = (Emp) o1;
                     Emp e2 = (Emp) o2;
                     return e1.getName().compareTo(e2.getName());
                  }
                  throw new ClassCastException("不能转换为Emp类型");
              }
          });
```

#  java.util.function 的方法

## Consumer：

提供无返回的函数操作，可以在方法中实现对数据的任何操作

```java
privatestaticvoidtestConsumer2(){
List<Integer>list=newArrayList<>();
Consumer<Integer>consumer=(s)->list.add(s+1);
Stream<Integer>stream=Stream.of(1,2,3,4,5,6,7,8,9);
stream.forEach(consumer);
System.out.println("---------------------");
list.forEach(System.out::println);
}
```



## Supplier

返回指定的类型，类似静态工厂方法，创建一个目标容器

 

```java
Suppelier<Integer> supplier = () -> new Random().nextInt();
supplier.get
```

 

### Predicate 

传入参数判断并返回boolean值

```java
Predicate<Integer> predicate = (t) -> t > 5
Predicate<String>predicate=(s)->s.equals("3");
Stream<String>stringStream=Stream.of("1","2","3","4","5");
Stream<String>stringStream1=stringStream.filter(predicate);
stringStream1.forEach(System.out::println);
```

 

## Function

用于转换数据获取子流

```java
Function<String,Integer>function=String::length;
Stream<String>stringStream=Stream.of("1","222","33","4444444","55555555555");
Stream<Integer>stream=stringStream.map(function);
```



# 流操作

相当于一种高级的迭代器，为java集合提供一种类似sql的集体操作，与常规集合不同的是他可以提供数据的修改

![合 / 其 他 数 据 淵  中 间  如 ： 选 ， 云  将 忮 要 求 转  成 犄 定 的 类 型 ](assets/clip_image001.png)

 

注意：流不能重复使用，一个流在赋值给一个变量后只能被使用一次

Stream<String>stream=Stream.of("1","","3","4","5");

这个流只能被使用一次，在使用一次后就会被关闭

原因是大部分流方法都会返回一个新的流，当前流会被关闭，类似于String的操作

```java
list=transactions.stream()
                 .filter(s->s.getYear()==2016)
                 .sorted(Comparator.comparing(Transaction::getValue))
                 .collect(Collectors.toList());
```

可以使用重用流来解决

使用：

1.创建流：

- Stream静态方法；其他类和对象的stream()方法

2.操作流

- filter(Predicate):Stream ：筛选出满足条件的元素
- distinct():Stream：去除相同的元素，根据hashCode和equals方法来判断
- limit(int):Stream：返回指定长度的前几个值
- skip(int):Stream：返回跳过前几个值的流
- sorted():Stream:自定义比较排序
- map(Function):Stream：把流转换成另一个流，一般用于提取

 

终端流：以下操作返回的不再是Stream对象

- anyMath(Predicate)boolean：检查流中是否存在满足条件的元素
- allMatch(Predicate)boolean：检查流中是否全部元素满足条件
- noneMatch(Predicate)boolean：检查流中是否全部元素不满足条件
- findAny()Optional：返回流中随机一个元素
- findFirst()Optional：返回流中第一个元素
- Collect(Collector):对流进行处理

[自定义controller学习](https://www.cnblogs.com/woshimrf/p/java8-learn-collector.html)

[Cllectors方法的学习](https://www.jianshu.com/p/f60b5800f61a)

- toCollection:把流中元素返回到一个集合Collection中
- toList:把流中元素返回到一个List中
- toSet:把流中元素返回到set中
- groupingBy：按照给定的属性为键分组返回
- partitioningBy：按照给定的判断条件对流中元素进行校验并返回一个map类型
- toMap:按照给定的键生成器和值生成器返回一个map
- reduce(BinaryOperator)Optional：把流元素按照某种运算规则组合起来



# 自定义collector

```java
public interface Collector<T, A, R> { 
//容器函数，生成结果容器
Supplier<A> supplier(); 
//累加器，执行具体的累加方法，每当访问一个元素时就执行累加器
BiConsumer<A, T> accumulator(); 
//当并行操作流时合并两个结果容器
BinaryOperator<A> combiner();
//对结果容器进行转换，返回成我们需要的容器
Function<A, R> finisher(); 
//对collector方法的规定，不设置会报异常 ,包含以下三个值
NORDERED--归约结果不受流中项目的遍历和累积顺序的影响
CONCURRENT--accumulator函数可以从多个线程同时调用，且该收集器可以并行归约流。如果收集器没有标为UNORDERED, 那它仅在用于无序数据源时才可以并行归约。
IDENTITY_FINISH--这表明完成器方法返回的函数是一个恒等函数，可以跳过。这种情况下，累加器对象将会直接用做归约过程的最终结果。这也意味着，将累加器A不加检查地转换为结果R是安全的。
Set<Characteristics> characteristics();
 }
T:要收集的对象的泛型
A:累加器在收集过程中用于累计结果的对象
R:收集操作返回的结果
```

 

例：实现列表自动排序成二级目录

```java
new Collector<Category, List<Category>, List<Category>>() {
    @Override
    public Supplier<List<Category>> supplier() {
        return ArrayList::new;
    }
    @Override
    public BiConsumer<List<Category>, Category> accumulator() {
        return (List<Category> listCategory,Category category)->{
            if(category.getFatherid()==0){
                System.out.println("father:"+category);
                listCategory.add(category);
                for(int i = list.indexOf(category);i<list.size();i++){
                    if(list.get(i).getFatherid()==category.getCategory_id()){
                        System.out.println("child:"+list.get(i));
                        listCategory.add(list.get(i));
                    }
                }
            }
        };
    }
    @Override
    public BinaryOperator<List<Category>> combiner() {
        return null;
    }
    @Override
    public Function<List<Category>, List<Category>> finisher() {
        return (i)->i;
    }
    @Override
    public Set<Characteristics> characteristics() {
        return Collections.unmodifiableSet(EnumSet.of(Characteristics.IDENTITY_FINISH));
    }
}

```

# lamdba表达式



使用“(参数)->{代码}”这种代码块来取代匿名类

```java
// Java 8之前：
new Thread(new Runnable() { 
@Override 
public void run() { 
System.out.println("Before Java8, too much code for too little to do"); } }).start(); 
//Java 8方式： 
new Thread( () -> System.out.println("In Java8, Lambda expression rocks !!") ).start();
```



格式：

(params) -> expression

(params) -> statement

(params) -> { statements } 

# Option

[学习链接](https://www.cnblogs.com/zhangboyu/p/7580262.html)

> 类似于一个容器

用途：解决为了维护空指针异常（NullPointerException）而产生的多余代码

```java
String isocode = user.getAddress().getCountry().getIsocode().toUpperCase();
```

如果为了避免空指针异常，则需要做以下处理

```java
if (user != null) {
     Address address = user.getAddress();
     if (address != null) {
         Country country = address.getCountry();
         if (country != null) {
             String isocode = country.getIsocode();
             if (isocode != null) {
                 isocode = isocode.toUpperCase();
             }
         }
     }
 }
```

 

使用Optional来避免空指针异常

```java
Optional<User> opt = Optional.ofNullable(user);
```

 

使用orElse来在值为空时返回另一个值

```java
User result = Optional.ofNullable(user).orElse(user2);
```

 

map() 对值应用(调用)作为参数的函数，然后将返回的值包装在 Optional 中

```java
String email = Optional.ofNullable(user)
       .map(u -> u.getEmail()).orElse("default@gmail.com");
```



# 线程池

[学习链接](https://baijiahao.baidu.com/s?id=1651975178610990562&wfr=spider&for=pc)

使用场景：单个任务处理时间段而任务数量大

![提 交 任 务  心 线 糧 数 达  任 务 队 列 已 满 ？  程 数 达 到 鼠  程 数 ？  执 行 和 銘  创 建 菲 核 心 线  程 执 行 任 务 ](assets/clip_image001-1583766967124.png)

 

父接口ExecutorService，有许多继承的接口实现了别的线程池管理功能，具体根据源码的继承实现来学习 

 

## java内部常用的线程池：

FixedThreadPool，SingleThreadExecutor，CachedThreadPool

 

支持自定义的线程池：ThreadPoolExecutor

- 4个核心参数：
- 核心线程数：80 20原则
- 任务队列长度：核心线程数/单个任务执行时间*2
- 最大线程数：（最大任务数-任务队列长度）*单个任务执行时间
- 最大空闲时间：

 

别的参数：

拒绝策略：

- AbortPolicy(抛出一个异常，默认的)
- DiscardPolicy(直接丢弃任务)
- DiscardOldestPolicy（丢弃队列里最老的任务，将当前这个任务继续提交给线程池）
- CallerRunsPolicy（交给线程池调用所在的线程进行处理)

 

 

## 自定义线程池

1.编写任务类，实现Runnable接口

2.编写线程类，执行任务

3.编写线程池类，包括提交任务，执行任务的功能



ExecutorService

- shutdown():停止接受新任务并执行当前所有的任务
- shutdownNow()：马上停止任务
- submit()：执行任务

Executors工厂类：创建线程池

 

callable和future

callable和runnable区别在于方法能够返回值

 

使用线程池的提交任务方法会返回一个future对象

get方法阻塞当前线程并等待返回结果

 

```java
public class ThreadTaskId implements Runnable {

    private final int id;

    public ThreadTaskId(int id) {
        this.id = id;
    }

    @Override
    public void run() {
        for (int i = 0;i < 5;i++) {
            System.out.println("TaskInPool-["+id+"] is running phase-"+i);
            try {
                TimeUnit.SECONDS.sleep(1);
                System.out.println("TaskInPool-["+id+"] is over");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
public class ThreadPoolExample {

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        for (int i = 0; i < 5; i++) {
            executorService.execute(new ThreadTaskId(i));
        }
        executorService.shutdown();
    }
}
```

