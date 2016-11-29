## emmet表达式
 
* ### **标签嵌套**:  
	1. > ：在下一级添加元素	
	2. + ：在同级上添加元素 
	3. 	^ ：在嵌套上提升一级
	4. * ：重复添加的次数
	5. () ：用于层级复杂时  
	
	`(header>div>ul>li.item$*5)+footer` 

	```
	<header>
	   <div>
	     <ul>
	       <li class="item1"></li>
	       <li class="item2"></li>
	       <li class="item3"></li>
	       <li class="item4"></li>
	       <li class="item5"></li>
	     </ul>
	   </div>
	 </header>
	 <footer></footer>
	
	```

* ### **属性**：#	 .	[]  
`div.container#contain[data-value]`  
  
	```
	<div class="container" id="contain" data-value=""></div>
	
	```

* ### **生成补全文字**：  
`lorem+单词个数+ tab`  
* ### **数字递加**：   
  `ul>li.item-$*5`
 
  ``` 
  <ul>
     <li class="item-1"></li>
     <li class="item-2"></li>
     <li class="item-3"></li>
     <li class="item-4"></li>
     <li class="item-5"></li>
   </ul>  
  ```
  
* ### **标签内文字**：{ }  
`p{内容内容}`  

	```
	<p>内容内容</p>
	```
* ### **css中使用**：  
```
.container{
  w + tab   ==> width: 
  bt        ==> border-bottom:
  m10 		==> margin: 10px;
  m10-20 	==> margin: 10px 20px;
  bdrs10 	==> border-radius: 10px;
  df		==> display: flex
}
```
* ### **注意事项**：  
 1. 书写时不能加空格
 2. 光标需要定位在末尾
 
  
 

