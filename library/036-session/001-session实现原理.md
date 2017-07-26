###  http://www.cnblogs.com/xdp-gacl/p/3855702.html  
### session 原理

服务器创建一个session之后 会把session的 id 号以cookie的形式写给客户机
用户不关闭浏览器 只要再去访问都会带着session的 id号去 服务器发现客户端带session id来了
就会从内存里面去找到对于session

也就是说创建session 之后往cookie中插入

```
//获取session的Id
String sessionId = session.getId();
//将session的Id存储到名字为JSESSIONID的cookie中
Cookie cookie = new Cookie("JSESSIONID", sessionId);
//设置cookie的有效路径
cookie.setPath(request.getContextPath());
response.addCookie(cookie);

```

如果cookie被禁止了 可以通过重写encodeURL的方法，使得session生效
