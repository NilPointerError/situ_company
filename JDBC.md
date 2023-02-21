# JDBC

利用java代码连接数据库，对数据库中的数据进行增删改查

### 连接数据库的六个步骤

1. 加载驱动类

```java
Class.forName("com.mysql.cj.jdbc.Driver");  //MysQL8.0版本含有cj
```

2. 创建连接

```java
String url="jdbc:mysql://localhost:3306/mydata?useSSL=false&characterEncoding=utf8";
String user="root";
String password="123456";
Connection con=DriverManager.getConnection(url, user, password);	
```

3. 获取执行对象

```java
Statement sta=con.createStatement();
String sql="insert into t_log values('jdbc')";
```

4. 执行sql语句

```java
int count = sta.executeUpdate(sql);		//增删改操作用executeUpdate()
```

5. 解析结果

```java
System.out.println(count);
```

6. 关闭连接

```java
sta.close();
con.close();
```

### JDBC工具类的封装

通过反射实现多种类的查询

```java
public static List query(String sql,Class clazz) {
		List list=new ArrayList();
		try {
			rs=sta.executeQuery(sql);
			Student stu=null;
			//获取类对象中所有的属性
			Field[] f_arr=clazz.getDeclaredFields();
			//设置所有属性可访问
			for(Field f:f_arr) {
				f.setAccessible(true);
			}
			//解析结果集-- 循环数据库的结果集
			while(rs.next()) {
				//一行数据就是一个对象(Class指定的对象)
				//实例化对象
				Object obj = clazz.newInstance();
				for(Field f:f_arr) {
					String fname=f.getName();//属性名
//					System.out.println(fname);
					Object data=rs.getObject(fname);
					f.set(obj, data);	//实例化对象赋值,每次赋一个属性值
				}
				list.add(obj);
			}
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return list;
	}
```

### sql.Date 与util.Date的区别？

sqlDate用来封装数据库中Date类型对应的数据,这个类型只记录日期,没时间
sqlTime可以获得时间

```java
Date date=new Date();
System.out.println(date);//Fri Jul 29 20:15:48 CST 2022

SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String datestr=sdf.format(date);
System.out.println(datestr);//2022-07-29 20:15:48
//怎样记录时间的
//java使用一个long类型值来记录当前时间 1970-1-1 0点开始计时 每过一毫秒+1
date=new Date(0);
datestr=sdf.format(date);
System.out.println(datestr);//1970-01-01 08:00:00

date=new Date();
long l=date.getTime();//毫秒数
int h=date.getHours();
int m=date.getMonth();
int y=date.getYear();
int d=date.getDay();
System.out.println(d);//5
		
```

### Statement 和PreparedStatement

![img](https://uploadfiles.nowcoder.com/images/20220210/927157434_1644473008677/DC7D2F09EF5C3133E1820320C0D75536)

Statement: 没预编译，不能防止sql注入

PreparedStatement: 预编译，语义不会发生改变，可防止sql注入

**语法**：

1.statement

```java
Statement stmt = connect.createStatement();
String sql= "SELECT * FROM cg_user WHERE userId=10086 AND name LIKE 'xiaoming'";
ResultSet rs = stmt.executeUpdate(sql);
```

2.preparedstatement

```java
PreparedStatement preparedStatement = connect.prepareStatement("SELECT * FROM cg_user WHERE userId= ? AND name LIKE ?");  
preparedStatement .setInt(1, 10086 );  
preparedStatement .setString(2, "xiaoming");  
preparedStatement .executeUpdate();  
```

**访问数据库的速度**

prepareStatement会先初始化SQL，先把这个SQL提交到数据库中进行预处理，多次使用可提高效率。
createStatement不会初始化，没有预处理，没次都是从0开始执行SQL

PreparedStatement对象不仅包含了SQL语句，而且大多数情况下这个语句已经被预编译过，因而当其执行时，只需DBMS运行SQL语句，而不必先编译。当你需要执行Statement对象多次的时候，PreparedStatement对象将会大大降低运行时间，当然也加快了访问数据库的速度。
这种转换也给你带来很大的便利，不必重复SQL语句的句法，而只需更改其中变量的值，便可重新执行SQL语句。选择PreparedStatement对象与否，在于相同句法的SQL语句是否执行了多次，而且两次之间的差别仅仅是变量的不同。如果仅仅执行了一次的话，它应该和普通的对象毫无差异，体现不出它预编译的优越性。

### [数据库连接池](https://blog.csdn.net/congchp/article/details/122884165)



