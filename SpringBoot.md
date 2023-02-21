控制反转 依赖注入优缺点

git常用命令

boot搭建

IOC DI  bean的作用域

单例模式

gc是一种守护线程

![image-20220810161426988](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220810161426988.png)

1、用户发起请求（Request），请求前端控制器；

2、由前端控制器请求处理器映射器，查询Handler（可以通过xml配置的形式查找或者注解的形式查找）；

3、处理器映射器向前端控制器返回处理器执行器链对象；

4、由前端控制器请求处理器适配器，请求执行Handler；

5、处理器适配器选择处理器执行Handler；

6、处理器向处理器适配器返回ModleAndView对象；

7、处理器适配器向前端控制器返回ModleAndView对象；

8、由前端控制器请求视图解析器，解析视图；

9、视图解析器向前端控制器返回View对象；

10、由前端控制器执行页面渲染；

11、由前端控制器向用户返回相应结果



Spring MVC 工作流程

深拷贝、浅拷贝
