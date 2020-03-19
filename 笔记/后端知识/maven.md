# maven环境下的多模块开发

[学习链接](https://blog.csdn.net/baidu_41885330/article/details/81875395https://blog.csdn.net/qq_28929589/article/details/79267467)

修改内容

1.修改父工程的打包方式为pom，修改打包使用的插件，声明所有的子模块

2.声明父模块，不需要打包的模块删掉build，开发时需要哪个模块导入哪个模块

3.要打包的模块上需要制定mainclass