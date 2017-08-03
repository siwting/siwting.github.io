Controller:  标识一个Spring类是Spring MVC controller处理器;
RestController:  是Controller和ResponseBody的结合体，两个标注合并起来的作用;

## 示例

``` java
 @Controller
 @ResponseBody
 public class ControllerA { }

 @RestController
 public class ControllerA { }

```

## 项目中遇到的问题

在开发用来给下拉框赋值的微信信息查询功能的时候，给Controller添加的注释写成了：
@Controller但是没有添加@ResponseBody,结果导致出现了一个很奇葩的问题，前台会
一直请求后台，而且每次都请求成功了Σ( ° △ °|||)︴，经历了一番痛苦的思索之后，
发现是WxInfoAction的SpringMvc注解添加错误了，只写了一个@Controller，迅速改
成@RestController之后，OK了；

**注解有风险，使用需谨慎！！**
