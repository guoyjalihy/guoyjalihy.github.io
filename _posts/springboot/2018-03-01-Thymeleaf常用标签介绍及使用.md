---
layout: post
title:  "Thymeleaf手册"
date:   2018-03-01 9:31:01 +0800
categories: springboot
---
[官方文档](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)

### 先上一个示例
```html
<li  th:each="menu:${session.menus}" th:with="isOpen=${session.currMenu}?${session.currMenu.parent == menu.id || session.currMenu.id == menu.id }:false" th:class="${isOpen}?active:''">
    <a th:if="${menu.url ne null}" th:id="|menu${menu.id}|"  th:href="|${menu.url}?menuId=${menu.id}|" ><i th:class="|fa ${menu.image}|"></i> <span class="nav-label" th:text="${menu.name}"></span></a>
    <a th:if="${menu.url eq null}" th:id="|menu${menu.id}|"  th:href="|${menu.url}?menuId=${menu.id}|" ><i th:class="|fa ${menu.image}|"></i> <span class="nav-label" th:text="${menu.name}"></span><span class="fa arrow"></span></a>
    <ul th:class="|nav nav-second-level collapse ${isOpen}?in:''|" th:if="${menu.subMenus ne null}" th:each="subMenu:${menu.subMenus}">
        <li th:class="${session.currMenu.id == subMenu.id}?active:''"><a th:id="|menu${subMenu.id}|" th:data-parent="${subMenu.parent}" th:href="|${subMenu.url}?menuId=${subMenu.id}|" th:text="${subMenu.name}"></a></li>
    </ul>
</li>
```
### 标准表达式语法
- 简单表达：
  - 变量表达式： ${...}
  - 选择变量表达式： *{...}
  - 消息表达式： #{...}
  - 链接网址表达式： @{...}
  - 片段表达式： ~{...}
  - 常用th标签都有那些？
- 文本操作：
  - 字符串连接： +
  - 文字替换： '|The name is ${name}\' 
- 比较和等于：
  - 比较：>，<，>=，<=（gt，lt，ge，le）
  - 等于操作符：==，!=（eq，ne）
- 条件操作
  - If-then: (if) ? (then)
  - If-then-else: (if) ? (then) : (else)
  - Default: (value) ?: (defaultvalue)  
```html
'User is of type ' + (${user.isAdmin()} ? 'Administrator' : (${user.type} ?: 'Unknown'))
```  

### 常用标签
- th:id 替换id    　　　　  
    ```html
    <input th:id="'xxx' + ${collect.id}"/>
    ```
- th:text 文本替换

  ```html
  <p th:text="${collect.description}">description</p>
  ```
　　　　
- th:utext    支持html的文本替换   

  ```html
  <p th:utext="${htmlcontent}">conten</p>
  ```
- th:object    替换对象    　　　　

  ```html
  <div th:object="${session.user}">
  ```
- th:value    属性赋值    　　　　

  ```html
  <input th:value="${user.name}" />
  ```
- th:with    变量赋值运算    　　　　

  ```html
  <div th:with="isEven=${prodStat.count}%2==0"></div>
  ```
- th:style    设置样式    　　　　　　　　

  ```html
  th:style="'display:' + @{(${sitrue} ? 'none' : 'inline-block')} + ''"
  ```
- th:onclick    点击事件    　　　　　　

  ```html
  th:onclick="'getCollect()'"
  ```
- th:each    属性赋值    　　　　　　　　

  ```html
  tr th:each="user,userStat:${users}">
  ```
- th:if    判断条件    　　　　　　　　

  ```html
  <a th:if="${userId == collect.userId}" >
  ```
- th:unless    和th:if判断相反    　　　　

  ```html
  <a th:href="@{/login}" th:unless=${session.user != null}>Login</a>
  ```
- th:href    链接地址    　　　　　　　　　　

  ```html
  <a th:href="@{/login}" th:unless=${session.user != null}>Login</a> />
  ```
- th:switch    多路选择 配合th:case 使用    

  ```html
  <div th:switch="${user.role}">
  ```
- th:case    th:switch的一个分支    　　　　

  ```html
  <p th:case="'admin'">User is an administrator</p>
  ```
- th:fragment    布局标签，定义一个代码片段，方便其它地方引用    

  ```html
  <div th:fragment="alert">
  ```
- th:include    布局标签，替换内容到引入的文件    

  ```html
  <head th:include="layout :: htmlhead" th:with="title='xx'"></head> />
  ```
- th:replace    布局标签，替换整个标签到引入的文件    

  ```html
  <div th:replace="fragments/header :: title"></div>
  ```
- th:selected    selected选择框 选中    

  ```html
  th:selected="(${xxx.id} == ${configObj.dd})"
  ```
- th:src    图片类地址引入    　　　　　　

  ```html
  <img class="img-responsive" alt="App Logo" th:src="@{/img/logo.png}" />
  ```
- th:inline    定义js脚本可以使用变量    

  ```html
  <script type="text/javascript" th:inline="javascript">
      [[${session.user.name}]];
  </script>
  ```
- th:action    表单提交的地址    　　　　

  ```html
  <form action="subscribe.html" th:action="@{/subscribe}">
  ```
- th:remove    删除某个属性    　　　　

  ```html
  <tr th:remove="all"> 
    1.all:删除包含标签和所有的孩子。
    2.body:不包含标记删除,但删除其所有的孩子。
    .tag:包含标记的删除,但不删除它的孩子。
    4.all-but-first:删除所有包含标签的孩子,除了第一个。
    5.none:什么也不做。这个值是有用的动态评估。
  ```
　　　　　　　　　　　　　　　　　　　　
- th:attr    设置标签属性，多个属性可以用逗号分隔   

  ```html
  th:attr="src=@{/image/aa.jpg},title=#{logo}"，此标签不太优雅，一般用的比较少。
  ```
  
- th:utext 非转义文本：unes​​caped text
  
  ```html
  #home.welcome=Welcome to our <b>fantastic</b> grocery store! 
  #执行此模板，默认使用<p th:text="#{home.welcome}"></p>来解析，结果为：
    <p>Welcome to our &lt;b&gt;fantastic&lt;/b&gt; grocery store!</p>
   #使用<p th:utext="#{home.welcome}"></p>即可。
    <p th:utext="#{home.welcome}">Welcome to our grocery store!</p>
  ```
  
- th:href  @{xxx} ：链接url的表达式

  ```html
    <a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>
  ```
  
- th:object 配合*{}使用

    ```html
      <div th:object="${session.user}">
        <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
        <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
        <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
      </div>
    ```  
    等同于：
    ```html
    <div>
      <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
      <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
      <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
    </div>
    ```
- ${{...}} 数据转换/格式化 
     ```html
    <td th:text="${{user.lastAccessDate}}">...</td>
    ```   

### 内置工具类
- \#execInfo：有关正在处理的模板的信息。
- \#messages：在变量表达式中获取外化消息的方法，与使用＃{...}语法获取的方法相同。
- \#uris：转义部分URL / URI的方法
- \#conversions：用于执行已配置的转换服务的方法（如果有）。
- \#dates：java.util.Date对象的方法：格式化，组件提取等。
- \#calendars：类似于#dates，但java.util.Calendar对象。
- \#numbers：格式化数字对象的方法。
- \#strings：String对象的方法：contains，startsWith，prepending / appending等。
- \#objects：一般的对象的方法。
- \#bools：布尔评估的方法。
- \#arrays：数组的方法。
- \#lists：列表方法。
- \#sets：集合的方法。
- \#maps：地图的方法。
- \#aggregates：在数组或集合上创建聚合的方法。
- \#ids：处理可能重复的id属性的方法（例如，作为迭代的结果）。

 示例如下(具体可参考[这里](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects))：
```html
 <li  th:each="menu:${session.menus}" th:id="|menu${menu.id}|" th:data-url="${#uris.escapePath(menu.url)}">
```

