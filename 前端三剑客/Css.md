# CSS介绍

#### 什么是CSS

- 层叠样式表
  + 层叠: 样式层层叠加 eg:刷墙 
  + 样式表: 样式的集合

> 学习html就是学习标签, 学习CSS主要学习样式(属性),选择器

#### CSS的作用

+ 美化页面,修饰页面  
+ HTML负责内容(hello),CSS负责样式(颜色,字体大小...)

```html
<font color="red" size="7">hello</font>
```

+ html当做毛坯房, CSS当做装修

#### 为什么要学习CSS  

- 我们在上次课中已经遇到了一些样式的问题, font标签的字体不能比1还小不能比7还大, 超链接标签的下划线去不掉, 大量进行嵌套来设置样式(eg: 段落里面嵌套font, 在font里面再设置color属性)

- 通过标签来修改样式的缺点:

  ​	1.需要记忆哪些标签有哪些属性, 如果该标签没有这个属性, 那么设置了也没有效果

  ​	2.当需求变更时我们需要修改大量的代码才能满足现有的需求

- 所以在企业开发中修改样式都是交给CSS来做,通过CSS来修改样式的好处:

  ​	1.不用记忆哪些属性属于哪个标签

  ​	2.当需求变更时我们不需要修改大量的代码就可以满足需求

  #### CSS语法

  ```
  选择器{
  	属性:属性值 属性值;
  	...
  	属性:属性值 属性值
  }
  ```

  **注意**

  - 属性和属性值用:连接
  - 如果有多个属性值用空格隔开
  - 如果有多个属性,属性和属性之间用;隔开  最后一个;可以不写

  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02_CSS概述和体验</title>
    <style>
        font[color]{
            color: blue;
        }

        font[size]{
            font-size: 100px;
        }

        a,font{
            text-decoration: line-through;
        }
        p{
            color: red;
        }
        div{
            border: 1px red dashed;
        }
    </style>
</head>
<body>
<div>
    div
</div>
<!--为什么要学css-->
<!--把所有的font标签的color属性值改成蓝色:大量修改代码,维护性太差  css可以解决问题-->
<font color="red">字体标签</font>
<font color="red">字体标签</font>
<font color="red">字体标签</font>
<font color="red">字体标签</font>
<font color="red">字体标签</font>
<font color="red">字体标签</font>
<br/>

<!--font标签中size属性的取值范围:1-7  如果想要让字体更大,就实现不了   css可以解决问题-->
<font size="7">字体</font>
<br/>

<!--a标签的下划线无法去除   css可以解决问题-->
<a href="#">百度</a>
<br/>

<!--段落标签,如果想要设置段落标签中的文本设置为红色,就必须嵌套font标签,如果不想嵌套,实现不了   css可以解决问题-->
<p>段落</p>

</body>
</html>
```

