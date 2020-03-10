> 这是一个快捷的html语法生成工具，输入相关指令再按tab键即可快速生成相关代码 

### 1.支持id选择器和css选择器

```html
div#test

div.test
```

 

### 2，子节点（>），兄弟节点(+)，父节点(^)

```html
div>ul>li>p

div+ul+p

div>ul>li^div
```

 

### 3.重复指令（*）

```html
div*5
```

 

### 4，分组（ （） ）

```html
div>(ul>li>a)+div>p
```

括号内容为单独的一个嵌套块，不受外部影响

 

### 5.属性（[attr]）

```html
a[href=’###’ name=‘xiaoA’] 
```

 

### 6.编号（$）

```html
ul>li.test$*3  $代表一位数
```

自定义从几开始递增：@

```html
ul>li.test$@3*3
```

 

### 7，文本（{}）

```html
ul>li.test$*3{测试$} 
```

 

### 8.一些隐式标签，比如能从父标签推断的子标签

```html
.test

ul>.test$*3

select>.test$*5
```



li：用于 ul 和 ol 中

tr：用于 table、tbody、thead 和 tfoot 中

td：用于 tr 中

option：用于 select 和 optgroup 中