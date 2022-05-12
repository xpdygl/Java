# 会话的概念

会话浏览器和服务器之间的多次请求和响应。

**用户打开浏览器，浏览不同的网页(资源)，发出多个请求，直到关闭浏览器的过程，称为一次会话(多次请求).** 



#### 常用的会话技术

##### cookie

​	==**cookie是客户端（浏览器）端的技术**==，用户浏览的信息以键值对(key=value)的形式保存在浏览器上。如果没有关闭浏览器，再次访问服务器，会把cookie带到服务端，服务端就可以做响应的处理。

##### session  

​	==**session是服务器端的技术**。==服务器为每一个浏览器开辟一块内存空间，即session。由于内存空间是每一个浏览器独享的，所有用户在访问的时候，可以把信息保存在session对象中。同时，每一个session对象都对应一个sessionId，服务器把sessionId写到cookie中，再次访问的时候，浏览器会把cookie（sessionId）带过来，找到对应的session对象。

cookie存在浏览器中。

session存在服务器中。

cookie和session可以互相发送转换

# cookie

### 常用api

```java
new Cookie(String name,String value); 创建一个Cookie对象(cookie只能保存字符串数据。且不能保存中文)
response.addCookie(cookie对象);  把cookie写回浏览器 
request.getCookies() ;得到所有的cookie对象。是一个数组，开发中根据key得到目标cookie中保存的值
cookie.getName() ; 获得cookie中设置的key
cookie.getValue(); 获得cookie中设置的value
```



### 默认路径



+ setPath(String url) ;设置路径,如果不设置路径,就是默认路径

通过设置不同的路径，可以控制我取到我想要的cookie(通过路径取)



cookie的路径通常设置为项目的部署路径. 当前项目下的Servlet都可以使用该cookie. 一般这么设置: cookie.setPath(==request.getContextPath()==);



取cookie

request.getCookies();

### 相同的key

遇到相同的key,后面的key会覆盖掉前面的key。

一定要保存同名的key就要设置两个不同的保存路径。通过cookie.setpath()



### cookie的有效时长

默认情况下, 当浏览器进程结束(浏览器关闭,会话结束)的时候，cookie就会消失。Cookie的编码

要设置有效期可以使用以下api

`cookie.setMaxAge(int expiry)`  :参数是时间,单位是秒

cookie的值为0 就代表删除cookie

cookie的值为-1就代表是默认级别的有效时长

正整数：以秒为单位保存数据有有效时间（把缓存数据保存到磁盘中）    





### 编码

cookie中的key默认不支持中文，以后都不使用中文

出现中文时设置一下浏览器和服务器的默认编码方式

把iso的编码方式换成utf-8。



URLEncode.encode(value,"utf-8");//存入的时候(先通过utf-8编码)
URLDecode.decode（value，"utf-8"）;//取出 (通过utf-8解码)

```
//处理请求和响应的乱码
request.setCharacterEncoding("utf-8");
response.setContentType("text/html;charset=utf-8");
```



### 封装工具类(获得指定的cookie)

3. 我们一般根据cookie的name获得目标cookie对象.  我们可以把这块逻辑封装到工具类里面

- 工具类

```java

/**
 * @Author：xuhui
 * @Date: 2021/5/6 10:07
 */
public class CookieUtils {

    /**
     * 查找指定的cookie
     * @param cookieName
     * @param cookies
     * @return cookie对象
     */
    public static Cookie getCookie(String cookieName,Cookie[] cookies){
        // 如果没有携带cookie
        if (cookies == null){
            return null;
        }

        // 如果有携带cookie,找到了
        for (Cookie cookie : cookies) {
            String name = cookie.getName();
            if (cookieName.equals(name)){
                // 找到了
                return cookie;
            }
        }

        // 如果有携带cookie,没找到
        return null;
    }

}
```



```java
package com.ithaima.demo5_记录上一次访问时间案例;

import com.ithaima.utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Date;

/**
 * @author XuHui
 * @version 1.0
 */
@WebServlet("/demo13Second")
public class Servletdemo13Second extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 0.处理响应乱码
        response.setContentType("text/html;charset=utf-8");
        // 1.获取存储上一次访问时间的目标Cookie
        Cookie[] cookies = request.getCookies();
        Cookie cookie = CookieUtils.getCookie(cookies,"lastTime");
        // 2.如果没有目标Cookie,就说明是第一次访问本网站
        if (cookie==null) {
            // 2.1 创建Cookie对象,存储当前系统时间
            Date date = new Date();
            long time = date.getTime();
            Cookie cookie1 = new Cookie("lastTime",time+"");
            // 2.2 设置cookie的存活时间和有效路径
            cookie1.setPath(request.getContextPath());
            cookie1.setMaxAge(360*7*24);

            // 2.2 响应Cookie到浏览器
            response.addCookie(cookie1);
            // 2.3 向页面输出提示信息: 您是第一次访问本网站
            response.getWriter().write("恭喜陈洁第一次变成🐖宝宝");
        }else {
            // 3.如果有目标Cookie,就说明不是第一次访问本网站
            // 3.1 获取目标Cookie中存储的上一次访问时间

            String value = cookie.getValue();
            Date lastDate = new Date(Long.parseLong(value));
            String lastTime = lastDate.toLocaleString();
            // 3.2 创建Cookie对象,存储当前系统时间
            Date date = new Date();
            long time = date.getTime();
            Cookie cookie1 = new Cookie("lastTime", time + "");
            // 3.3 设置cookie的存活时间和有效路径
            cookie1.setPath(request.getContextPath());
            cookie1.setMaxAge(360*7*24);
            // 3.4 响应Cookie到浏览器,覆盖之前的cookie(键相同,路径相同就会覆盖)
            response.addCookie(cookie1);
            // 3.4 把取出来的上一次访问时间,显示到页面上
            response.getWriter().write("陈洁上一次变成🐖宝宝的时间是"+lastTime);
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}

```

启动tomcat 输入正确路径就可以访问了

删除cookie要到浏览器里面删 删完之后就可以重新记录时间了

# Session

### 常用api

​	==范围: 一次会话(多次请求)== 保存用户各自的数据(以浏览器为单位)   

- request.getSession(); 获得session(如果第一次调用的时候其实是创建session,第一次之后通过sessionId找到session进行使用)
- Object getAttribute(String name) ;获取值
- void setAttribute(String name, Object value) ;存储值
- void removeAttribute(String name)  ;移除值,只是移除键值对,并不会删除session对象





作为域对象存取值

作用在一次会话中

一次会话可以有多个请求

#### cookie和Session的不同

- cookie是保存在浏览器端的，大小和个数都有限制。session是保存在服务器端的， 原则上大小是没有限制(实际开发里面也不会存很大的数据), 安全一些。    
- cookie不支持中文，并且只能存储字符串；session可以存储基本数据类型，集合,对象等

#### Session的执行原理

​	**1.浏览器请求服务器,会携带cookie**

​	**2.服务器就会从cookie中获取sessionId**

​	**3.判断是否有sessionId,**

​	**4.如果没有,就直接为该浏览器创建新的Session对象(request.getSession() --->没有就创建,有就获取)**

​    **5.如果有sessionId,就根据sessionId查找对应的session对象**

​    **6.如果能找到session对象,那就直接使用**

​    **7.如果找不到session对象(session对象销毁),就直接创建新的session对象**









#### 关于Session对象的销毁

- 浏览器关闭了, session使用不了, 是session销毁了吗?
  - session没有销毁,只是浏览器端的含有sessionId的cookie没有了.  
  - session基于cookie, sessionId保存到cookie里面的, 默认情况下cookie是会话级别,浏览器关闭了cookie就是消失了,也就是说==sessionId消失了==, 从而找不到对应的session对象了, 就不能使用了.
- 结论:
  - 结论:  
    - 1.服务器响应的sessionId,是使用的默认级别的Cookie,关闭浏览器,cookie销毁了,也就是sessionId没有了,但session对象还在服务器中
    - 2.服务器响应的sessionId是持久级别的Cookie,关闭浏览器,cookie还在,也就是sessionId还在,并且session对象也还在服务器中, 可以获取上一次会话保存在session中的数据

- **解决: 自己获得sessionId, 自己写给浏览器 设置Cookie的有效时长, 这个Cookie的key必须: `JSESSIONID**`

```java

/**
 * @Author：xuhui
 * @Date: 2021/5/6 15:02
 */
@WebServlet("/ServletDemo21")
public class ServletDemo21 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建session对象存值
        HttpSession session = request.getSession();

        // 获取sessionId
        String id = session.getId();
        System.out.println("ServletDemo21...id:"+id);

        // 存值
        session.setAttribute("akey","aaa");

        // 手动响应sessionId
        Cookie cookie = new Cookie("JSESSIONID",id);
        // 设置有效时长
        cookie.setMaxAge(60*60);
        // 设置有效路径
        cookie.setPath(request.getContextPath());
        // 响应cookie
        response.addCookie(cookie);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }
}


```

**真正的销毁session对象,得使用session对象的invalidate()方法*





### 三个域对象比较

| 域对象             | 创建                                          | 销毁                                                         | 作用范围       | 应用场景                                                |
| ------------------ | --------------------------------------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------- |
| ServletContext     | 服务器启动                                    | 服务器正常关闭/项目从服务器移除                              | 整个项目       | 记录访问次数,聊天室                                     |
| HttpSession        | ==没有session 调 用==request.getSession()方法 | session过期（默认30分钟）/调用invalidate(）方法/服务器==异常==关闭 | 会话(多次请求) | 验证码校验, ==保存用户登录状态==等                      |
| HttpServletRequest | 来了请求                                      | 响应这个请求(或者请求已经接收了)                             | 一次请求       | servletA和jsp（servletB）之间数据传递(转发的时候存数据) |

#### 三个域对象怎么选择? 

三个域对象怎么选择? 

​	一般情况下, 最小的可以解决就用最小的.

​	但是需要根据情况(eg: 重定向, 多次请求, 会话范围, 用session;  如果是转发,一般选择request)