
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
```   
$('#div1').attr('name', 'hello');
alert($('#div1').attr('name')); // hello   
 
//相当于原生中的  setAttribute，getAttribute：
document.getElementById('div1').setAttribute('name', 'hello');  
alert(document.getElementById('div1').getAttribute('name'));  //hello  

----------------------------------------------  

$('#div1').prop('name', 'hello');
alert($('#div1').prop('name')); // hello   

//相当于原生中的：
document.getElementById('div1').['name'] = 'hello';  
alert(document.getElementById('div1').['name']);  //hello  

----------------------------------------------    
 
$('#div1').data('name', 'hello');
alert($('#div1').data('name')); // hello     
 
```  
- 内存泄漏产生的情况之一是 ：DOM元素和对象之间互相引用，大部分浏览器就会出现内存泄漏  
如果出现以下情况：浏览器就会出现内存泄漏  

	```  
	var oDiv = document.getElementById('div1');  
	var obj = {};  
	oDiv.name = obj;  
	obj.age = oDiv;  
  
	```  

  $.data可以解决以上问题:  
    
  ```  
  $('#div1').data('name', obj);  
  ```  
     
  此时不管name设置的是不是对象，或者obj是不是引用了 $('#div1')这个元素，都不会发生内存泄漏  
    
### Object.defindeProperty()  
```  
var obj = { name : 'hello'};  
Object.defindeProperty(obj, 0, {  
	get: function() {  
		return {};
	}
});  
//相当于 var obj = { name : 'hello', 0 : {}}; 不设置set方法则其对应属性不可以被修改  


alert(obj[0]);  //[object Object]
obj[0] = 123;  
alert(obj[0]);  //[object Object]  
``` 
  
### $.queue   
- 针对animation  
- 针对多异步的顺序管理，比 $.deferred 更强大  
    
###  .attr 与.prop 区别与联系 
```   
$('#div1').attr('name', 'hello');
alert($('#div1').attr('name')); // hello   
 
//相当于原生中的  setAttribute，getAttribute：
document.getElementById('div1').setAttribute('name', 'hello');  
alert(document.getElementById('div1').getAttribute('name'));  //hello  

----------------------------------------------  

$('#div1').prop('name', 'hello');
alert($('#div1').prop('name')); // hello   

//相当于原生中的：
document.getElementById('div1').['name'] = 'hello';  
alert(document.getElementById('div1').['name']);  //hello  

```  
- 区别一：.prop定义的自定义属性 ，无法在标签中显示出来，但是确实已经定义了，可以获取到 
  
```   
$('#d1').attr('gaolin', 'hao');   
//<div id="d1" gaolin="hao"></div>   
   
$('#d1').prop('gaolin', 'hao');     
//<div id="d1"></div>  
alert($('#d1').prop('gaolin')); //hao  
 
```    
- 区别二： .prop无法获取自定义属性  
   
```   
 <div id="d1" gaolin="hao"></div>   
 $('#d1').attr('gaolin') //hao  
 $('#d1').prop('gaolin') //空无结果  
  
```   
- 区别三： 针对a标签的表现   
 
```  
<a href="haha.com"></a>   
$('a').attr('href') // haha.com  
$('a').prop('href') // 返回本地的全路径，如：file:///E/asdfsafsdf/haha.com  
```  
- 区别四：  
  
```  
<input type="checkbox"/>  

alert( $('input').attr('checked') )  //checked  
alert( $('input').prop('checked') )  // true  

```

在jq中已经为类似属性做了兼容，如一下这两种都可以实现功能：  
  
```  
$('input').attr('checked', 'true')   
$('input').attr('checked', 'checked')  
  
```   
### .addClass  
  
```    
$('div').addClass(function(index){
	return 'div'+index;
}) 


<div class="div0"></div>  
<div class="div1"></div> 
<div class="div2"></div> 
```  
### &&比||的优先级高   
alert(1 || 0 && 2) // 1  先执行 0&&2  再执行 1||0
   




  
