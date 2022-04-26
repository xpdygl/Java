## restful风格

@restController



## 表现层数据封装

+ ①：设定http请求动作（动词）【增加&修改】
+ ②：设定请求参数（路径变量）【根据id 删除&查询】
+ ③：设定http请求动作（动词）【查询所有】



创建一个结果模型类Result，将要响应的数据放进Result的data属性中。

response.data = result对象



### 结果类



```java
public class Result
{
  private Object data;   	//后台要响应的数据
  Private Integer code;		//成功码或失败码
  private String msg;			//提示信息
}
```

实际开发中 后端给前端的数据是统一格式的

+ 统一定义封装类（data code msg）
+ 定义code常量

### 状态码

将data放到状态码中 状态码code代表要进行的操作

比如20021代表删除

```json
"code":20021,
  "data"true:
```

