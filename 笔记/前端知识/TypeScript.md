

# TypeScript

个人认为最大的作用在于变量和方法的约束，方便项目的管理

# 在变量名后的几个特殊符号

!/as：断言该变量必须为指定的属性

```typescript
let strLength: number = (someValue as string).length;
```

此处断定someValue这个变量为string类型

?:用于属性定义，表示这个属性要么是指定的属性，要么是undefined

```typescript
interface SquareConfig {    color?: string;    width?: number;}
```

|：或，表示变量的类型为指定的这几种

```
var val:string|number 
```