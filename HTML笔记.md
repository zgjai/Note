#标签释义
1. \<h1\>--\<h6\> 标题
2. \<p\>  段落
3. \<a\>  链接  
	* href属性，值为链接的目标
	* target属性，定义链接的文档在何处打开，如_blank表示在新的标签页中打开
4. \<img\> 图像  
	* alt属性，用于规定图像代替文本
	* src属性，用于显示图像的url
	* height属性，定义图像的高度
	* width属性，定义图像的宽度
	* usemap属性，表示使用\<map\>标签
5. \<br /\> 空元素  
6. \<hr /\> 水平线
7. 文本格式化标签
	* \<b\> 粗体
	* \<big\> 大号字
	* \<em\> 着重
	* \<i\> 斜体
	* \<small\> 小号
	* \<strong\> 加重语气
	* \<sub\> 下标字
	* \<sup\> 上标字
	* \<ins\> 插入字，表现为下划线
	* \<del\> 删除字，表现为删除线
	* \<abbr\> 简写标签，鼠标靠近时会展示出完整版本，即title的值
8. \<table\> 表格
	* cellpadding属性，单元格边距
	* cellspacing属性，单元格间距
	* bgcolor属性，背景颜色
	* background属性，背景图像
	* align属性，单元格内容的位置
	* border属性，定义表格的边框，为0时无边框
	* \<tr\>包含表格一行的数据\</tr\>
	* \<td\>包含一个单元格的数据，可以为空\</td\>
	* \<th\>表头，该标签被包含于\<tr\>标签中
		* colspan属性，值为2表示横跨2列单元格
		* rowspan属性，值为2表示横跨2行单元格
	* \<caption\>表格标题
9. \<ul\> 无序列表
10. \<ol\> 有序列表
11. \<li\> 列表项的开始标签
12. \<dl\> 自定义列表
	* \<dt\> 自定义列表项的开始标签
	* \<dd\> 自定义列表项的定义
13. \<form\> 表单元素  
	* \<input\> 输入标签  
		* type属性，输入的类型
			* text,文本框
			* radio,单选框
			* checkbox,复选框
			* submit,提交按钮，点击后会执行action属性中的动作 

14. \<iframe\> 内联框架  
	* src属性，指向内联的页面
	* height属性，定义iframe标签的高度
	* width属性，定义iframe标签的宽度
	* frameborder属性，定义iframe标签的边框，为0时表示移除iframe的边框
	* name属性，可以用于让链接显示在内联页面中


#具体实例
* **使标题成为链接**  

		<h1>    
			<a href="www.taobao.com"> This is a link </a>       
		</h1>
		
* **html中插入样式表**

	外部样式表（在\<head\>中使用）：  
	
		<link rel="stylesheet" type="text/css" href="mystyle.css">

	内部样式表（在\<head\>中使用）： 
	 
		<style type="text/css">    
			css代码
		</style>
		
	内敛样式：  

		<p style="color:red; margin-left:20px"> 使用该样式的段落 </p>
	
* **创建图像映射,即在图像中创建可点击区域**    

		<img src="planets.jpg" border="0" usemap="#planetmap" alt="Planets" />    
		<map name="planetmap" id="planetmap">  
			<area shape="circle" coords="180,139,14" href="venus.html" alt="Venus" />
			<area shape="rect" coords="0,0,110,260" href="sun.html" alt="Sun" />
		</map> 			

	
