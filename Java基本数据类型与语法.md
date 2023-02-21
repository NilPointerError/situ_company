# 7月8号
> JDK: 开发工具包
> > 包含JRE、JVM、编译工具、调试工具


## 配置环境变量
* JAVA_HOME: jdk的安装路径
* Path
  * 可执行文件的命令
  * JDK可执行文件（bin目录）
  * %JAVA_HOME%\bin
* CLASSPATH 
  * 指定编译文件在哪个位置
  * .;%JAVA_HOME%\bin 


## JDK目录
bin: 可执行文件的目录

include: c语言头文件

jre: java程序的运行环境

lib: Java包，存放一些常规的类

## 验证
* javac &nbsp; 在Path中找可执行文件
* java -version 查看jdk看版本
* javac Hello.java 编译Java文件，编译成字节码文件.class
* java Hello 执行类中的主方法
* set 临时配置环境变量
## 基本数据类型

![数据类型](C:\Users\Admin\Desktop\Java Notes\pic\2.png)

|序号|数据类型|位数|默认值|取值范围|举例说明|
|:---:|:---:|:---:|:---:|:---:|:---:|
|1|byte|8|0|-2<sup>7</sup>~2<sup>7</sup>-1|`byte b = 127;`|
|2|short|16|0|-2<sup>15</sup>~2<sup>15</sup>-1|short s = 10;|
|3|int|32|0|-2<sup>31</sup>~2<sup>31</sup>-1|int i = 10;|
|4|long|64|0|-2<sup>63</sup>~2<sup>63</sup>-1|long l = 10l;|
|5|float|32|0.0|-2<sup>128</sup>~2<sup>128</sup>|float f = 12.2f;|
|6|double|64|0.0|-2<sup>1024</sup>~2<sup>1024</sup>|double d = 12.22;|
|7|char|16|空|0~2<sup>16</sup>-1|char c = 'c';|
|8|boolean|8|false|true; false|boolean bool = true;|
## 默认类型
* 整数类型的默认类型 int
* 浮点型的默认类型 double
## 进制
```java
//二进制
i = 0b11;
//八进制
i = 011;
//十六进制
i = 0x11;
```
## 类型转换
- 隐式转换
  - 整数类型间的转换（取值范围小的类型可以直接转换为取值范围大的类型）
  - 字符类型和整数类型的转换
    - 65535
- 强制类型转换
  - 基本类型强制转换（二进制截取）
## 浮点数

- 存储方式

  - float

  - double

    ![img](https://img-blog.csdn.net/20180307151704981)


![](https://img-blog.csdn.net/20140106125430875?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemRsNTQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center "optional title")

- [数制转换](https://blog.csdn.net/zdl543/article/details/17915379)
## Java基本语法
- 变量&常量
  - 变量
    - 程序运行过程中可能会发生变化的量
    - 声明和赋值
    - 初始化
    - 标识符命名规则
      1. 变量名只能包含字母数字_$,首字符不能是数字
      2. 变量名不能是关键字
      3. 见名知意
      4. 尽量使用驼峰命名法
    - 类名：首字母大写
    - 包名：全部小写
    - 常量名：全部大写
- 运算符
  - 算数运算符
    - \+ - * / %
  - 关系运算符
    - \> < >= <= == !=
  - 逻辑运算符
    - && ||
  - 赋值运算符
    - += -= *= /= %= 
- 分支判断
  - if...else... 
  - switch...case...default
