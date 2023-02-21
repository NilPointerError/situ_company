## JS

文件的引用

```js
<script src="js/index.js"></script>
```

变量的声明

```js
a=12;// var a=12;   //全局变量
let b=12;			//局部变量,不可提升作用域
```

函数的声明

```js
function fun(){
    a=12;
    let b=12;
    a=a+23;
    b+=23;
}
fun();
console.log(a);//35
console.log(b);//12
```

分支判断

```js
a=12;
if(a%2==0){
	console.log('偶数');
}else{
	console.log('奇数');
}
```

循环语句

```js
for(var i=0;i<10;i++){
	console.log(i);
}
```

引用类型

```js
var obj={};
console.log(obj);//Object {  }
obj.name="张三";
console.log(obj);//Object { name: "张三" }
obj.name="李四";
obj.sex="女";
console.log(obj);//Object { name: "李四", sex: "女" }
delete obj.sex;
console.log(obj);//Object { name: "李四" }
```

数组类型

```js
var arr=new Array();//空数组
arr=new Array("123","345");//初始化 Array [ "123", "345" ]
arr=new Array(6);//声明长度为6的数组

var arr2=[];
arr[15]=23;    //js中数组长度可变,长度为16
```

数组操作

```js
var arr=[1,2,3,4,5,6,7];
arr.push(8);//追加元素
arr.pop();//删除最后一个元素
arr.unshift("hello");//从前端加入一个
arr.shift("");		//删除第一个元素
arr.splice(1,2);	//删除指定位置的元素
console.log(arr);//Array(5) [ 1, 4, 5, 6, 7 ]
arr=[1,2,3,4,5,6,7];
arr.splice(1,2,"你好","js soeasy");
console.log(arr);//Array(7) [ 1, "你好", "js soeasy", 4, 5, 6, 7 ]
```

函数引用

```js
f();//f
var f=function(){//这种方式的声明f需放在函数下才执行
    console.log("a")
}
//前置声明,在代码运行之前已经声明完成
function f(){
    console.log("f");
}
```

函数的调用

```js
function fun(a,b){
	var sum=0;
	for(var i=0;i<arguments.length;i++){
		sum+=arguments[i];
	}
	return sum;
}
			
var sum=fun(12,23,34,45,56);//函数形参数可变
console.log(sum);//170
```

使用js DOM获取标签对象

```js
<div id='demo'></div>
<script>
    //选择id为demo的全部标签
    var demo=document.getElementById("demo");
    console.log(demo);//<div id="demo">
	var arr=document.getElementsByName("easy");    
	console.log(arr);//NodeList []
	arr=document.getElementsByTagName("div");
	console.log(arr);//HTMLCollection { 0: div#demo, length: 1, … }
	arr=document.getElementsByClassName("fix");	
	console.log(arr);//HTMLCollection { length: 0 }
	//dom获取第一个被选中的组件
	arr=document.querySelector("div");
	console.log(arr)//<div id="demo">
	arr=document.querySelectorAll("div");
	console.log(arr)//NodeList [ div#demo ]	
```

设置页面内容

```js
var demo=document.getElementById("demo");
demo.innerText="<java so easy>";
demo.innerHTML="<h1>java so easy</h1>";
demo.style.border="5px solid red";
console.log(demo);
demo.remove();
</script>
```

## jQuery

使用jQuery可简化开发

jQuery 是js库，定义的一系列js函数

```js
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			.color-red{
				color: red;
			}
			
		</style>
	</head>
	<body>

		<!-- 简化开发 -->
		<input type="text" name="username"/>
		<button type="button" id="btn1">点击</button>
		
		<div id="demo"></div>
		<script src="js/jquery-3.6.0.min.js"></script>	
	
		<script>
			var dom=document.getElementById("demo");
			dom.innerText="nihao";
			$("#demo").text("你好");
			var t=$("#demo").text();
			console.log(t);
			$("#btn1").click(function(){
               var v=$("[name='username']").val();
                console.log(v);
                //this 是一个dom对象
                $(this).addClass("color-red");
            });
		</script>
		
	</body>
</html>
			
```

## Ajax

ajax作用:在页面不跳转的情况下刷新数据

```js
<button type="button">获取数据</button>
<script src="js/jquery-3.6.0.min.js"></script>
<script>
    var obj={name:'zhangsan',sex:'male'};
	console.log(JSON.stringify(obj));
		$("button").click(function(){
            console.log("1------");//异步执行
            $.ajax({
               url:'json/data.json',
                data:{},
                success:function(d){//成功后传回data.json中的数据
                    console.log("3-----");
                    console.log(d);
                    $("button").before("<h1>"+d.name+"</h1>");
                }
            });
            console.log("2-----");//执行顺序 1、2、3  ajax发送数据到后台返回回来运行较慢
        });

```







