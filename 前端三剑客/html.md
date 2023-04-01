# html

html是超文本语言

可以用来设计网页 制作网页。

html由标签组成。每个标签都要有开头和结尾

结尾要带有斜杆。

#### HTML语法规范

- 扩展名是html或者htm


- html标签不区分大小写


- html由头(head)和体(body)组成 
  + 头标签里一般放title和配置。


- 标签是可以嵌套的,标签里面可以放标签  


- 标签一般由起始标签开始，结束标签终止(成对出现)。但是如果标签不修饰内容，可以在标签里结束.  
- <标签名/> 空标签



#### 标签属性

- 属性是属于标签的，修饰标签，让标签有更多的效果
- 属性一般定义在起始标签里面。 

- 属性一般以 **属性名=属性值**的形式存在
- 属性值一般用 `''` 或者`“ ”` 括起来。 

```html
例如
<head>
  
</head>
<body>
  
</body>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_快速入门</title>
</head>
<body>
<font color="red" size="5" face="微软雅黑">hello world...</font>
</body>
</html>
```



### 图片标签

```html
<img src="图片的路径" width="宽" height="高" alt="图片描述" title="鼠标放上去展示的信息"/>
<img src="../img/tv01.jpg"> <font size="3">TCL电视, 品质保障</font>
```

2. 路径问题
   + 绝对路径(以http开始的, 以盘符开始的)
   + 相对路径
     + ./    当前目录
     + ../  上级目录



### 超链接标签

```html
超链接标签的格式:
		<a href="指定需要跳转的目标路径" target="打开的方式">需要展现给用户查看的内容</a>
		target属性取值: 
            _blank：新起页面
            _self：当前页面（默认）
```

在a标签的括号内写链接  href



### 列表标签

+ 无序列表



```html
<ul type="类型">
    <li>需要显示的条目内容</li>
    ...
</ul>
 <!--type属性: 列表的类型; circle: 空心圆; square: 实心的正方形-->
```



```html
<ul type="circle\square\默认实心圆">
	<li></li>
</ul>
```



+ 有序列表

```html
<ol type="" start="">
	<li></li>
</ol>
```

> li定义ol或者ul里面 内容定义在li里面



### 表格标签

##### 3.1.1 基本表格

###### 语法

* 表格：由`<table>`标签定义；
* 行：每个表格里有若干行`<tr>`；
* 单元格：每行被分割为若干单元格`<td>`。
  * 单元格里可以包含文本、图片、列表、段落、表单、水平线、表格等

这些标签可以嵌套使用， tr 中可以设置长宽高的像素 



### 旅游首页练习

#####   分析

1. 创建8行的表格
2. 第1行(顶部图片): 定义img
3. 第2行(搜索): 嵌套1行3列的表格
4. 第3行(菜单): 嵌套1行9列的表格, 设置背景色, 每个格子里面定义超链接(居中)
5. 第4行(轮播图): 定义img
6. 第5行(线路部分): 嵌套3行4列的表格, 进行格子合并
7. 第6行(线路部分): 嵌套3行4列的表格, 进行格子合并
8. 第7行(底部图片): 定义img
9. 第8行(文字): 定义文字, 居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
<!--1.创建一个8行1列的表格-->
<table  width="98%" align="center">
    <!--2.第1行嵌套一个图片标签-->
    <tr>
        <td width="100%" height="100px">
            <img src="../img/top_banner.jpg" width="100%" height="100px">
        </td>
    </tr>
    <!--3.第2行嵌套一个1行3列的表格-->
    <tr>
        <td width="100%" height="100px">
            <table width="100%" height="100px">
                <tr>
                    <td><img src="../img/logo.jpg"></td>
                    <td><img src="../img/search.png"></td>
                    <td><img src="../img/hotel_tel.png"></td>
                </tr>
            </table>
        </td>
    </tr>
    <!--4.第3行嵌套一个1行9列的表格-->
    <tr>
        <td width="100%" height="40px">
            <table width="100%" height="40px" >
                <tr bgcolor="#FFC900" align="center">
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                    <td><a href="#">首页</a></td>
                </tr>
            </table>
        </td>
    </tr>
    <!--5.第4行嵌套一个图片标签-->
    <tr>
        <td width="100%" height="400px">
            <img src="../img/banner_1.jpg" width="100%" height="400px">
        </td>
    </tr>
    <!--6.第5行嵌套一个3行4列的表格-->
    <tr>
        <td width="100%" height="550px">
            <table width="100%" height="550px">
                <tr width="100%" height="50px">
                    <td colspan="4">
                        <img src="../img/icon_6.jpg">
                        <font>国内游</font>
                    </td>
                </tr>
                <tr>
                    <td rowspan="2" width="25%" height="500px">
                        <img src="../img/guonei_1.jpg" width="100%" height="500px">
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                </tr>
                <tr>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                </tr>
            </table>
        </td>
    </tr>
    <!--7.第6行嵌套一个3行4列的表格-->
    <tr>
        <td width="100%" height="550px">
            <table width="100%" height="550px">
                <tr width="100%" height="50px">
                    <td colspan="4">
                        <img src="../img/icon_6.jpg">
                        <font>国内游</font>
                    </td>
                </tr>
                <tr>
                    <td rowspan="2" width="25%" height="500px">
                        <img src="../img/guonei_1.jpg" width="100%" height="500px">
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                </tr>
                <tr>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                    <td width="25%" height="250px" align="center">
                        <img src="../img/jiangxuan_1.jpg">
                        <p>上海直飞三亚5天4晚自由行(春节预售...</p>
                        <font color="red">¥899</font>
                    </td>
                </tr>
            </table>
        </td>
    </tr>
    <!--8.第7行嵌套一个图片标签-->
    <tr>
        <td width="100%" height="80px">
            <img src="../img/footer_service.png" width="100%" height="80px">
        </td>
    </tr>
    <!--9.第9行嵌套一个字体标签-->
    <tr>
        <td width="100%" height="10px" align="center">
            <font color="gray" size="2">江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved 苏ICP备16007882</font>
        </td>
    </tr>
</table>

</body>
</html>
```

实现图片滚动轮播

```java
```





### 表单标签（重点）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_表单标签元素</title>
</head>
<body>
<!--
表单:表单是一个包含表单元素标签的区域。
作用:用来提交数据到服务器(后台)
表单标签: form
表单元素标签:
    input标签:输入框
        type属性: 类别
            text:文本输入框
            password:密码输入框
            radio:单选框
            checkbox:复选框
            file:上传文件
            submit:提交按钮
            reset:重置按钮
            button:按钮
            hidden:隐藏

    select标签:下拉框
        option标签:下拉选项
    textarea标签:文本域
        cols属性: 列数
        rows属性: 行数
-->
<form>
    隐藏属性:<input type="hidden" value="1"><br/>
    用户名:<input type="text"><br/>
    密码:<input type="password"><br/>
    性别:<input type="radio">男 <input type="radio">女<br/>
    爱好:<input type="checkbox">篮球
         <input type="checkbox">足球
         <input type="checkbox">看电影
         <input type="checkbox">敲代码<br/>
    图像:<input type="file"><br/>
    籍贯:<select>
            <option>深圳</option>
            <option>广州</option>
            <option>东莞</option>
            <option>惠州</option>
        </select><br/>
    自我介绍:<br/>
        <textarea cols="20px" rows="15px">

        </textarea>
        <br/>
    <input type="submit">
    <input type="reset">
    <input type="button" value="空白按钮">

</form>

</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>超级赛亚人</title>
    <script>
        alert("陈洁大聪明")
    </script>
</head>
<!--
表单：在网页中主要负责数据采集功能，使用<form>标签定义表单
表单项(元素)：不同类型的 input 元素、下拉列表(select)、文本域(textarea)等
input输入框的类型:使用type属性来设置
    type属性值:
        text: 文本输入框
        password:密码输入框
        radio: 单选框
        checkbox:复选框
        file: 文件选择项
        submit:提交表单数据按钮
        reset:重置表单数据
        button:普通按钮
        hidden:隐藏域---隐藏域不会显示到页面----一般用来存储唯一标识
-->
<body>

        <form>
            用户名     <input type="text" value="aaa"><br/>
            password<input type="password" name="password" value="123"><br/>
            sex <input type="radio" name="sex" value="男">男<br/>
            sex <input type="radio">女<br/>
            hobby<input type="checkbox" checked> 篮球
            <input type="checkbox"> 坤坤
            <input type="checkbox"> music
            <input type="checkbox" checked> sleep<br/>
            file <input type="file"><br/>
            籍贯 ：
            <select>
                <option>霞浦</option>
                <option value="home" selected>盐田</option>
                <option>长乐</option>
                <option>香港</option>
            </select>
            自我介绍<br/>
            <textarea cols="20" rows="15"></textarea><br/>
            <input type="submit">
            <input type="reset">
            <input type="button" value="空白按钮">
        </form>

</body>
</html>
```

check是默认选择这个元素的意思，一打开这个网页就默认选择这个选项 不过用户可以更改

input里面用name来默认选择

select里面用selected来默认选择



