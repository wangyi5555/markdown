# 代理模式

## 原理

> 为其他对象提供一个代理以控制对某个对象的访问。代理类主要负责为委托了（真实对象）预处理消息、过滤消息、传递消息给委托类，代理类不现实具体服务，而是利用委托类来完成服务，并将执行结果封装处理。 

举例：婚庆公司和夫妻都继承了结婚接口实现了结婚方法，夫妻实现的是出席婚礼，而婚庆公司实现了整套流程，过程中调用夫妻的结婚方法来完成流程（有点像aop）

## spring

[aop](https://blog.csdn.net/weixin_40160543/article/details/92010760)和事务

包括jdk动态代理和cglib动态代理

jdk：利用反射方式生成一个实现代理接口的匿名类，然后在调用具体方法前调用invokeHandler来处理

cglib：使用asm开源包，对代理对象的class文件加载进来，修改其字节码继承生成字类来处理

spring中如果有顶层接口（即使用接口实现的方式编写的业务类），则默认使用jdk,可强制修改成cglib,如果直接是一个类则必须使用cglib代理



区别：

jdk代理只对实现接口的类生成代理，而不能针对类

cglib只针对类生成代理，通过继承的方式来实现，所以类不要声明为final



# @AutoWired和@Resource

autowired：根据类型，默认要求被注入的对象是必须存在的

resource：根据名称



autowired中出现多个类？

使用@Qualifier 来指定要注入哪个类

