
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
  
### $.noConflict()  
```  
var $ = 123;   
var gaolin = $.noConflict();  
gaolin(function(){  
	alert($); // 123  
})   

var jQuery = 456;   
var gaolin = $.noConflict(true);  
gaolin(function(){  
	alert(jQuery); // 456  
})  
```
### $.Callbacks
```  
var list = $.Callbacks();  
  
list.add(function(){console.log(1)});
list.add(function(){console.log(2)});
list.add(function(){console.log(3)});
list.add(function(){console.log(4)});  
   
list.fire(); // 1,2,3,4  
```    
可以对不同作用域的函数进行统一管理  
  
```  
var cbs = $.Callbacks();    
function aaa(){  
	alert(1);
}   
cbs.add(aaa);  
(function(){  
 	function bbb(){  
 		alert(2)
 	}   
 	cbs.add(bbb); 
 	
 })()   
 cbs.fire();// 1, 2
```  
### $.Defferred()  

- 以下执行结果相同  

```  
var cb = $Callbacks();  
setTimeout(function(){   
	alert(1);   
	cd.fire()  
},1000);   
cb.add(function(){  
	alert(222)  
})  


var dfd = $Defferred();  
setTimeout(function(){   
	alert(1);   
	dfd.resolve()  
},1000);   
dfd.done(function(){  
	alert(222)  
})    
```  
  
- 第一次延迟执行，接下来都将立即执行的时候用下面方法：  
  
```    
var dfd = $.Deferred();  
setTimeout(function(){  
	alert(111);  
	dfd.resolve();
},1000);   
 
dfd.done(function(){  
	alert('第一次延迟一秒执行');
});  
  
$('input').click(function(){  
	dfd.done(function(){  
		alert('以后都立即执行');
	})
})
```  
- $.when  
针对多个延迟对象  
  
```  
function aaa(){  
	var dfd = $.Deferred();  
	dfd.resolve();  
	return dfd;
}  
function bbb(){  
	var dfd = $.Deferred();  
	dfd.resolve();  
	return dfd;
}  
$.when(aaa(), bbb()).done(function(){  
	alert('a,b都成功了')//aaa,bbb都成功才执行done
}).fail(function(){  
	alert('a或者b失败')// aaa,或者bbb有一个失败就执行fail
})    
```

### 判断数据类型  ({}.toString.call())
```
var a = 'string';// a=1,a={},a=[]  
alert({}.toString.call(a)) //string,number,object,array  
```  

### 一般情况下判断相等都要用 ===  
只有以下两种情况用 == ：  
1. null == null  //true  
2. null == undefined // true


### !!   
!!是类型转换  将对应的类型转换为boolean型
### jQ可以获取display:none元素的属性，但是原生的js不可以  
  
```  
$('#div1').width() // 100  
$('#div1').get(0).offsetWidth  // 0   

<div id="div1" style="width:100px; height:100px; display:none"></div>  
 
```  
jq将“display:none”存起来，然后添加“display:block; visibiliti:hidden; position:absolute”，之后获取宽度值并保存，然后将样式再改成原始的“display:none”  

### $.data  

  
