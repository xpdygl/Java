

### 数据类型

定义变量用var 

```javascript
var test = 1;
var test2 = "打到"
var flag = true;
```

ECMAScript 6 新增了 let 关键字来定义变量。它的用法类似于 var，但是所声明的变量，只在 let 关键字所在的代码块内有效，且不允许重复声明

```javascript
{
  let num2 = 20
  comsole.log(num2)
}
comsole.log(num2)
```

ECMAScript 6 新增了 const关键字，用来声明一个只读的常量。一旦声明，常量的值就不能改变。 

### 常用方法

altert("打印内容到网址")

console.log 打印东西到控制台，在浏览器的开发者选项里可以查看

### 装换类型

parseint

parsefloat

parseboolean

+ 布尔类型转为数字类型

True 转为1

false转为0

```javascript
var flag2 = true
var flag3 = false
console.log(flag2+1); //2
console.log(flag3+1); //1
```



0和NaN转为false 其余的为true

String 空字符串转为false 其他转为true

null 转为false

Undefined 转为fasle



### 运算符

js中除法会保留小数

==		两个等于号比较值是否相等

===		3个等于号比较值和类型是否相等

number类型与字符串运算的时候，字符串会自动进行类型转换。，前提是字符串里面的数值满足number类型。



### 流程控制语句

if

Which 

for 

while

do while