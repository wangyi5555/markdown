# 基础命令

## 系统操作

ip addr :查看ip地址



sync :将数据从内存同步到硬盘中



shutdouwn:关机

​	-h + ==time==:指定关机的时间

​	 now:马上关机



reboot:重启

​	-r +==time==:指定重启时间

​	 now :马上重启

## 目录操作

cd :切换目录

​	 .. :返回上一级目录

​	~:返回当前用户目录

可以相对路径也可以绝对路径



pwd:显示用户当前所在目录



ls:列出当前目录下的文件

​	-a:查看包括隐藏文件的所有文件

​	-l:查看文件的权限和属性



mkdir+==name==:创建目录



rmdir+==name==:删除目录,只能删除空目录



cp +==old==+==new==:复制文件或者目录到指定的地方



rm:删除文件或者目录

​	-f:忽略不存在的文件

​	-r:递归删除目录

​	-i:询问是否删除



mv:移动文件或者目录

​	-f:强制删除

​	-u:只替换已经更新过的文件

## 权限操作

![1585146797648](assets/1585146797648.png)

分别对应owner group others三个属性

使用ls -l命令可以在每个文件查看到该文件的权限

权限为:文件所有者,文件所有者同组的用户,其他用户

chgrp ==groupname== ==filename== 修改文件的属组

​	-R:递归修改文件夹下的属组

chown ==owername== ==filename==:修改文件的所有者

chmod ==分数== ==filename==:修改文件的九个属性

分数计算方式为累加

分数分别为:

r:4

w:2
x:1

> 如-rwxrwx---
>
> owner:4+2+1=7
>
> group:4+2+1=7
>
> others:0+0+0=0

对应的命令为:chmod 770 filename

## 查看文件内容

cat ==filename==:从第一行开始显示文件

tac ==filename==:从最后一行开始显示文件

nl ==filename==:输出行号

more filename:分页显示文件

​	空格:翻页

​	回车:下一行

less filename:与more类似,但是可以往前翻页

​	pageup,pagedown

​	q:退出

​	/要查询的内容:向下查询

​	?要查询的内容:线上查询

​	n:向下查找下一个

​	N:向上查找上一个

head ==filename==:查看前几行

​	-n ==number==:指定行号

teil ==filename==:查看尾几行

## 文件操作

touch filename 创建文件

echo ==string== >>==filename==:输入字符串到指定文件

vi ==filename==:编辑文件

## vim编辑器

> 分为三种模式:命令模式,输入模式,底线命令模式

### 命令模式

刚进入文件即为命令模式

i:切换到输入模式

x:删除当前光标所在字符

:切换到底线命令模式

### 输入模式

按esc退出输入模式,切换到命令模式

### 底线命令模式

q:退出程序

w:保存文件

set nu:设置行号

## 用户管理

useradd ==username==

​	-m:创建用户的主目录

​	-G:分配组

userdel ==username== 删除用户

​	-r:删除用户的目录

usermod ==content== ==username== 修改用户



su username:切换用户



## 磁盘管理

df :列出文件系统整体磁盘使用量

du:检查磁盘使用量

mut ==outer== ==inner== :把外部文件挂载到指定目录下



## 进程管理

ps:查看正在运行的进程信息

​	-a:显示所有信息

​	-u:以用户信息显示

​	-x:显示后台进程参数

ps -xx|grep ==name==:查看指定进程

ps -ef:查看父进程

kill -9 ==id== :杀掉指定进程



# Shell脚本

可以在shell脚本中编写linux命令来批量执行

## 第一个基础的shell脚本

1. 创建文件：vi test1.sh
2. 编辑文件

```shell
#!/bin/bash #指定使用这个解析器
echo "Hello World !"
```



3. 执行：

```shell
chmod +x ./test1.sh
./test1.sh
```

./代表当前目录，如果不写系统会去PATH里找

## 变量

声明：

```shell
name="wangyi"
```

使用(只在使用时才需要)

```shell
echo ${name}
```

删除(不能删除只读变量)

```shell
unset name
```

shell脚本包括以下三种变量：

1. 局部变量：只在当前shell脚本上有效
2. 环境变量：所有的程序都能访问的变量
3. shell变量：由shell程序设置的特殊变量

## 字符串操作

双引号可以拼接字符串和变量

```shell
your_name='runoob'
str="Hello, I know you are ${your_name}! \n"
echo -e $str
```

获取字符串长度

```shell
string="abcd"
echo ${#string} #输出 4
```

截取字符串

```shell
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

查找字符位置

```shell
string="runoob is a great site"
echo `expr index ${string} io`  # 输出 4
```

删除字符串

```shell
string="a b c d e f g h i j"
${string#a*f} #从开始删除a-f的字符串
${string##a*} #从开始删除a以后的字符
${string%f*j} #从结尾删除f-j的字符
${string%%*j} #从结尾删除j前面的所有字符
```



## 数组（使用方法类似字符串）

定义

```shell
array_name=(value0 value1 value2 value3)
```

可以单独定义各个数组的变量，可以不使用连续的下标，下标的范围没有限制

读取数组所有内容

```shell
echo ${array_name[@]}
```

## 往脚本传参

使用$1，2，3....来读取传入的参数

```shell
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

执行

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
```

几个内置的特殊字符

| 参数 | 意义                                   |
| ---- | -------------------------------------- |
| $#   | 传递到脚本的参数个数                   |
| $*   | 以一个单字符串显示所有向脚本传递的参数 |
| $$   | 脚本运行的当前进程号                   |
| $!   | 后台运行的最后一个进程的id号           |
| $@   | 以多个字符串显示所有向脚本传递的参数   |
| $-   | 显示Shell使用的当前选项                |
| $?   | 显示最后命令的退出状况，除0外都有错    |

## 运算

```shell
#!/bin/bash
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

 注意：

1. 反引号
2. 操作符之间必须有空格

### 关系运算符

只支持数字，不支持字符串，除非字符串为数字

| 运算符 | 说明                             |
| ------ | -------------------------------- |
| -eq    | 判断两个数是否相等               |
| -ne    | 检测两个数是否不相等             |
| -gt    | 判断左边的数是否大于右边的数     |
| -lt    | 判断左边的数是否小于右边的数     |
| -ge    | 判断左边的数是否大于等于右边的数 |
| -le    | 判断左边的数是否小于等于右边的数 |

### 布尔运算符

| 运算符 | 说明   |
| ------ | ------ |
| ！     | 非运算 |
| -o     | 或运算 |
| -a     | 与运算 |

### 字符串运算符

| 运算符    | 说明                    |
| --------- | ----------------------- |
| -z        | 判断字符串长度是否为0   |
| -n        | 判断字符串长度是否不为0 |
| 直接$name | 判断字符串是否为空      |

### 文件测试运算符

| 操作符          | 说明                    |
| --------------- | ----------------------- |
| -b ==filename== | 文件是否为块设备文件    |
| -c              | 文件是否为字符设备文件  |
| -d              | 文件是否为目录          |
| -f              | 文件是否为普通文件      |
| -g              | 文件是否设置了SGID位    |
| -k              | 文件是否设置了粘着位    |
| -p              | 文件是否是有名管道      |
| -u              | 文件是否设置了SUID位    |
| -r              | 文件是否可读            |
| -w              | 文件是否可写            |
| -x              | 文件是否可执行          |
| -s              | 文件是否为空（大小为0） |
| -e              | 文件是否存在            |

### 内置命令

echo：输出字符串

```shell
echo string
```

read：从标准输入中读入并赋值到变量中

```shell
read name
```

printf：格式输出 类似c语言的格式

test：检测某个条件是否成立

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

### 流程控制

判断

1. if

```shell
if condition
then
   operate
fi
```

2. if else-if else

```shell
if condition1
then
    operate
elif condition2 
then 
    operate
else
    operate
fi
```

循环

1. for

```shell
for var in item1 item2 ... itemN
do
    operate
done
```

2. while

```shell
while condition
do
    operate
done
```

3.  until （直到该条件成立才停止）

```shell
until condition
do
    operate
done
```

选择

case

```shell
case value in
value1)
   operate
    ;;
value2）
    operate
    ;;
esac
```

## 函数

声明

```shell
[ function ] funname (...)
{
    action;
    [return int;]
}
```

1. function关键字可加可不加
2. return可以不加，不加将以最后一条命令结果作为返回值

调用

跟在外部文件调用类似

1. 当参数数量>=10时需要加上大括号${10}
2. 也可以使用文件传参定义的参数

## 输入输出重定向

| 命令            | 说明                                               |
| --------------- | -------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

## 导入外部脚本

```shell
. filename
```

注意之间有空格