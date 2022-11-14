# html5

#### HTML语法规范

- 扩展名是html或者htm


- html标签不区分大小写


- html由头(head)和体(body)组成 
- 标签是可以嵌套的,标签里面可以放标签  


- 标签一般由起始标签开始，结束标签终止(成对出现)。但是如果标签不修饰内容，可以在标签里结束.  
- <标签名/> 空标签



一些配置信息和标题放在head里面

文字信息放在body里面

#### 标签属性

- 属性是属于标签的，修饰标签，让标签有更多的效果
- 属性一般定义在起始标签里面。 

- 属性一般以 **属性名=属性值**的形式存在



1. 标题标签

```
<hn></hn>   n取值1~6
```

2. 段落标签  段落之间自动进行换行

```
<p></p>
```

3. 粗体标签

```
<b></b>
```

4. 斜体标签

```
<i></i>
```

5. 换行标签

```
<br/>
```

6. 下划线标签

```
<hr/>
```

#### 图片

标签是 <img></img>

中间添加图片的路径 

也可以将图片直接拖进idea 然后就会直接显示路径

语法:

```html
<img src="图片路径" width="宽" height="高" alt="图片描述" title="用于告诉浏览器, 当鼠标悬停在图片上时, 需要弹出的描述框中显示什么内容"/>
```



####  超链接

#####

+ 超链接标签的作用: 就是用于控制页面与页面(服务器资源)之间跳转的

```
超链接标签的格式:
		<a href="指定需要跳转的目标路径" target="打开的方式">需要展现给用户查看的内容</a>
		target属性取值: 
            _blank：新起页面
            _self：当前页面（默认）
```

+ 示例代码

```html
    <!--href属性: 跳转的路径(可以是本地的也可以是远程的)
        target属性: 链接打开方式; _blank: 新开一个窗口;_self:在当前页面打开(默认值)
    -->

    <a href="http://www.baidu.com" target="_self">百度</a>
    <a href="../案例一信息展示案例/index.html">案例一信息展示页面</a>
```



#### 无序列表

```html
<ul type="circle\square\默认实心圆">
	<li></li>
</ul>
```



#### 表格

* 由`<table>`标签定义
* 行：每个表格里有若干行`<tr>`；
* 单元格：每行被分割为若干单元格`<td>`。
  * 单元格里可以包含文本、图片、列表、段落、表单、水平线、表格等

![1568558602924](/Users/xuhui/Desktop/就业班资料/就业班作业/day21_html&css/01_笔记/img/1568558602924.png)

```html
<!--表格标签: table-->
<!--行标签: tr-->
<!--单元格标签: td-->
<!--注意:单元格里可以包含文本、图片、列表、段落、表单、水平线、表格等-->
<!--关系:table标签中嵌套tr标签,tr标签中嵌套td标签-->
<!--快捷键: table>tr*行的数量>td*列的数量 tab键-->
<!--eg:创建一个4行5列的表格-->
<!--
    table标签的属性:
         border:1表示有边框(像素), 0表示没有边框
         width: 宽度(像素,百分比)
         height:高度(像素,百分比)
         align:对齐方式  center:居中对齐  left:左对齐(默认)  right:右对齐
         cellspacing(了解):单元格和单元格之间的间隙
         cellpadding(了解):单元格内容和单元格之间的间隙
         bgcolor: 背景颜色(颜色单词,十六进制的颜色代码)
    tr标签属性:align:对齐方式  center:居中对齐  left:左对齐(默认)  right:右对齐
    td标签属性:align:对齐方式  center:居中对齐  left:左对齐(默认)  right:右对齐
              colspan: 合并列
              rowspan: 合并行
    th标签: 表示表头标签(默认居中对齐,自动加粗)
    caption标签: 设置表名(默认居中对齐)
```

### 单元格合并

colspan列合并

rowspan行合并

```html
<!--
  colspan: 合并列
  rowspan: 合并行
  套路:
    1.确定要合并的单元格
    2.删除所有要合并的单元格,留下最前面的那个单元格
    3.确定是跨列合并,还是跨行合并
    3.确定后,在留下的单元格中设置对应的属性,属性值为合并的单元格数量
-->
```

##### 表格容易错的地方

```html
<!--1.就算只有1行1列, td不能少-->
<table>
    <tr></tr>
</table>

<!--2.合并之前, 每一行的列的个数应该一样-->
<table>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
    </tr>
</table>

<!--3. table定义tr, tr里面定义td, td里面再放内容-->
<table>
    111
    <tr>
        222
        <td>
            333
        </td>
    </tr>
</table>
```

#### 旅游界面

##### 

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





#### 表单

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
```



#### form表单通用属性name和value【重点】

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02_form表单通用属性name和value</title>
</head>
<body>
<!--
表单元素标签的name属性:
    1.为单选框和复选框分组
    2.作为key提交数据到后台(服务器),后台(服务器)会以name属性的值作为键去获取对应的值,如果表单元素标签没有设置name属性,后台是无法获取表单标签中输入的值

表单元素标签的value属性:
    - 给按钮起名字
    - 设置提交到服务器的值  name=value
-->
<form>
    隐藏属性:<input type="hidden" value="1"><br/>
    用户名:<input type="text" name="username"><br/>
    密码:<input type="password" name="password"><br/>
    性别:<input type="radio" name="sex" value="boy">男 <input type="radio" name="sex" value="girl">女<br/>
    爱好:<input type="checkbox" name="hobby" value="basketball">篮球
    <input type="checkbox" name="hobby" value="football">足球
    <input type="checkbox" name="hobby" value="film">看电影
    <input type="checkbox" name="hobby" value="code">敲代码<br/>
    图像:<input type="file" name="fileName"><br/>
    籍贯:<select name="jiGuan">
            <option value="sz">深圳</option>
            <option value="gz">广州</option>
            <option value="dg">东莞</option>
            <option value="hz">惠州</option>
        </select><br/>
    自我介绍:<br/>
    <textarea cols="20px" rows="15px" name="text">

        </textarea>
    <br/>
    <input type="submit">
    <input type="reset">
    <input type="button" value="空白按钮">

</form>

</body>
</html>
```

##CSS

在head中写一个style，指定下方的标签 就可以一次性配置所有被指定到的标签

#### css语法

#### 

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



```css
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

