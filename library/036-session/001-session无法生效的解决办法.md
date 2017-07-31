###  http://www.cnblogs.com/xdp-gacl/p/3855702.html  
### session 无法生效的解决方案



#### 首先也看一下session是如何实现的

服务器创建一个session之后 会把session的 id 号以cookie的形式写给客户机
用户不关闭浏览器 只要再去访问都会带着session的 id号去 服务器发现客户端带session id来了
就会从内存里面去找到对于session

也就是说创建session 之后往cookie中插入

``` java
//获取session的Id
String sessionId = session.getId();
//将session的Id存储到名字为JSESSIONID的cookie中
Cookie cookie = new Cookie("JSESSIONID", sessionId);
//设置cookie的有效路径
cookie.setPath(request.getContextPath());
response.addCookie(cookie);

```

也就是如果cookie被禁止了 会导致session 失效，这个时候就要重写encodeURL的方法，使得session生效


``` java
    //  用于对sendRedirect方法后的url地址进行重写。
    response.encodeRedirectURL(java.lang.String url);
    // 用于对表单action和超链接的url地址进行重写
    response.encodeURL(java.lang.String url);

```
