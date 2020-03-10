# 新增的关键字let和const 

var：在function中是局部变量，在其他地方是全局变量，可以声明多次，并且能被变量提升

let：只在声明的代码块中有效，只能声明一次，不能被变量提升

const：声明之后即为常量，不可改变（保证指向的地址是不会变的）

# 解构赋值 

- Es5新增：剩余运算符和展开运算符：…

展开操作：

```javascript
let arr = ["蚂蚁部落", 6, "softwhy.com"];
console.log([...arr,"青岛市南区"]);
```

…把arr中的数据展开保存到这个新的数组中

 

剩余操作：

```javascript
let [a, ...b] = [1, 2, 3];
//a = 1
//b = [2, 3]
```



> 可以对对象和数组进行解构赋值，并且嵌套或者忽略某些值

## 数组：

 

默认：

```javascript
let [a, b, c] = [1, 2, 3];
// a = 1
// b = 2
// c = 3
```

 

嵌套：

```javascript
let [a, [[b], c]] = [1, [[2], 3]];
// a = 1
// b = 2
// c = 3
```

 

忽略：

```javascript
let [a, , b] = [1, 2, 3];
// a = 1
// b = 3
```

 

字符串也可以进行解构操作：

```javascript
let [a, b, c, d, e] = 'hello';
// a = 'h'
// b = 'e'
// c = 'l'
// d = 'l'
// e = 'o'
```

本质上是所有实现了Iterator接口的对象都可以进行解构

 

指定默认值

```javascript
let [a = 2] = [undefined];
 // a = 2
```

当存在undefined 时即触发默认值的设置

 

## 对象：

基本

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
// foo = 'aaa'
// bar = 'bbb'
```

剩下的与数组类似



# Map和Set 

Es6中新增的两个对象Map和Set

 

## Map：

与Object的区别：

1. map中的键可以是任何类型
2. 可以获取size属性来统计数量
3. map中键值对是有序的（FIFO）

 

创建：new map()可指定参数：Map或者Array

使用set(),get()设取值

 

方法：

迭代Map：

 

- for...of

```javascript
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
// 将会显示两个 log。 一个是 "0 = zero" 另一个是 "1 = one"
for (var [key, value] of myMap) {
  console.log(key + " = " + value);
}
```

 

- forEach()

```javascript
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");

// 将会显示两个 logs。 一个是 "0 = zero" 另一个是 "1 = one"
myMap.forEach(function(value, key) {
  console.log(key + " = " + value);
}, myMap)
```

 

与数组之间的转换：

```javascript
var kvArray = [["key1", "value1"], ["key2", "value2"]];
// Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
var myMap = new Map(kvArray);
// 使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组
var outArray = Array.from(myMap);
```

 

## set：存储唯一值的集合

使用new Set（）创建一个新的set

 

与数组之间的转换

```javascript
// Array 转 Set
var mySet = new Set(["value1", "value2", "value3"]);
// 用...操作符，将 Set 转 Array
var myArray = [...mySet];
```

String

```javascript
// String 转 Set
var mySet = new Set('hello');  // Set(4) {"h", "e", "l", "o"}
// 注：Set 中 toString 方法是不能将 Set 转换成 String
```

 

使用解构操作符和set组合可以完成以下作用

数组去重，并集，交集，差集

# String常用方法 

- includes():判断是否找到参数字符串
- startsWith()：判断参数字符串是否在目标字符串头部
- endsWith()：判断参数字符串是否在目标字符串尾部

可选参数有：String和Number 表示从指定位置开始判断

- repeat()：复制目标字符串指定次数
- match（）：匹配目标正则表达式并返回匹配的值
- replace（）：匹配目标正则表达式并替换成指定的值



# 数组常用方法 

- filter（Function）：迭代过滤出指定条件的目标
- find（Function）：迭代查找满足指定条件的第一个值
- findIndex(Function)：迭代查找满足指定条件的第一个值
- includes（）:判断数组是否包含某个指定的值
- push（）：向数组末尾插入一个值
- unshift（）：向数组开头插入一个值
- pop（）：删除数组的最后一个元素
- shift（）：删除数组的第一个元素
- splice（index，number，item）：在数组的第index个位置删除number个元素并在该位置插入item
- reduce（Function）：聚合数组的操作
- reverse（）：反转数组
- sort（function）：对数组排序

# 模块化编程

export和import

export：

包括export default和export

一个js文件只能有一个export default但可以有多个export

 

import：

导出的属性为只读，不可进行重新赋值