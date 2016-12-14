
### 1.constructor

```
jQuery.fn = jQuery.prototype = {
jquery: core_version,//$().jquery // 返回jquery的版本

constructor: jQuery,//修正constructor的指向，因为 jQuery.prototype = {}  
 这种写法，当你新建实例的时候，原型中自动生成的constructor会被覆盖，因此需要重新写  
 一下。jQuery.prototype.a = {},这种写法不会覆盖

}   
``` 
  

### 2.选择符  

```$('<li>', {title: 'hi',html: 'abcd'}).appendTo('ul')```  

### 3.ready写法 （文档加载）  
  
```  
$(function(){})   

$(document).ready(function(){})   
```     
### 4.makeArray和 toArray  
makeArray是工具方法，可以给jQuery类用，也可以给jQuery实例用 ；  
toArray 是在prototype中定义的，是实例方法，只能给jQuery实例用  
$('div').toArray返回的是一个原生的dom元素数组，因此不能使用jQuery的方法了   
 
### 5. $.extend = $.fn.extend = function(){...}   
  
1) 当只有一个对象自变量的时候，jq中扩展插件的形式  
  
```    
 //扩展工具方法
 $.extend({  
 	aaa: function(){  
 		alert(1);  
 	},  
 	bbb: function(){  
 		alert(2);  
 	}
 });  
 $.aaa() // 1  
 $.bbb() // 2  
 
  //扩展JQ实例方法  
 $.fn.extend({  
 	aaa: function(){  
 		alert(3);  
 	},  
 	bbb: function(){  
 		alert(4);  
 	}
 });   
  $().aaa();  // 3  
  $().bbb();  // 4   
```  
2) 当有多个对象自变量的时候，后面的对象扩展到第一个对象身上 
  
```  
var a = {};  
$.extend(a, {name: 'hello'}, {age: 30});  
console.log(a); // {name:hello,age:30};  
 
```  
3) 深拷贝和浅拷贝  
    
```    
//浅拷贝
var a = {};  
var b = { name = 'hello'};  
$.extend(a, b);  
a.name = 'hi';  
alert(b.name); //hello   
   
//深拷贝
var a = {};  
var b = { name : {age: 30}};  
$.extend(a, b);  
a.name.age = 20;  
alert(b.name); //20 b被修改了，此时需要深拷贝  
 
var a = {};  
var b = { name : {age: 30}};  
$.extend(true, a, b);  
a.name.age = 20;  
alert(b.name); //30 b未被修改   
    
```  
### 深拷贝和浅拷贝  
```   
//浅拷贝 
var obj = {  
	a: 10
};  
function copy(obj) {   
	var newObj = {};  
	for(var attr in obj){  
		newObj[attr] = obj[attr];
	}   
	return newObj;
}
var obj2 = copy(obj);  
obj2.a = 20;  
console.log(obj.a);  

//深拷贝 
var obj = {  
	a: {b: 10}
};  
function deepCopy(obj) {   
	var newObj = {}; 
	for(var attr in obj){    
		if(typeof attr === Object) {  
			deepCopy(attr);
		}else{  
			newObj[attr] = obj[attr];
		}
	}   
	return newObj;
}  
function deepCopy2(obj) {   
	if(typeof obj != 'object'){  
		console.trace(); 
		return obj;
	}  
	var newObj = {}; 
	for(var attr in obj){    
		newObj[attr] = deepCopy(obj[attr]);
	}   
	return newObj;
}  

var obj2 = deepCopy(obj);  
obj2.a.b = 20;  
console.log(obj);  
console.log(obj2);
console.log(obj.a.b);// 10  


  
```  
### 递归  
1. 函数调用函数自身  
2. 最后一次判断一个终止条件，跳出递归   
 
```    
function test(n){  
	if(n==1){    
		console.trace();
		return 1;
	}
	return n*test(n-1);
}  
alert(4) // 24
```  

  
### console.trace()  
console.trace()用来追踪函数的调用轨迹。  
比如，有一个加法器函数。 [console其他方法](http://www.ruanyifeng.com/blog/2011/03/firebug_console_tutorial.html) 

```  
function add(a,b){
　　　　return a+b;
　　}  
　　  
```  
我想知道这个函数是如何被调用的，在其中加入console.trace()方法就可以了。       

```  
　　function add(a,b){
　　　　console.trace();
　　　　return a+b;
　　}  
```  
假定这个函数的调用代码如下：  
  
  ```  
    var x = add3(1,1);
　　function add3(a,b){return add2(a,b);}
　　function add2(a,b){return add1(a,b);}
　　function add1(a,b){return add(a,b);}  
  ```

