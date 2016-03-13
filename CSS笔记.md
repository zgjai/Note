#CSS语义
1. css定义  
	层叠样式表，全称为Cascading Style Sheets

2. css组成  
	主要由两部分组成：选择器以及一条或多条声明。
	实例：  
	
		h1{
			color:blue;
			font-size:12px;
		}

3. id选择器  
	id选择器可以为标有特定id的HTML元素指定特定的样式，
	HTML元素通过id属性来设置id选择器，CSS中id选择器以"#"来定义。
	
4. class选择器
	class选择器用于描述一组元素的样式，class选择器不同于id选择器的地方在于，它可以作用于多个元素，
	class选择器在HTML中以class属性表示，在CSS中，class选择器以一个"."表示，需要注意的是class名的第一个字符不能用数字，否则某些浏览器不支持
	
5. 多重样式的优先级
	由高到低的顺序：  
	* 内联样式（及在HTML元素内部）
	* 内部样式表（位于\<head\>标签内部）
	* 外部样式表
	* 浏览器缺省设置

6. 背景属性  
	background，简写属性，属性值的顺序从上至下：
	* background-color，背景颜色
	* background-image，背景图片
	* background-repeat，设置背景图像是否及如何重复
		* repeat，默认值，水平垂直方向都会重复
		* repeat-x，只有水平位置会重复
		* repeat-y，只有垂直位置会重复
		* no-repeat，不会重复
		* inherit，从父元素继承 
	* background-attachment，设置图像是否固定或者随着页面的其余部分滚动  
		* scroll，默认值，表示会滚动
		* fixed，固定
		* inherit，从父元素继承
	* background-position，设置背景图像的起始位置

7. Text文本格式
	* color，文本颜色
	* text-align，文本的对齐方式
		* center，居中
		* right，对齐到右端
		* left，对齐到左端
		* justify，每一行被展开为宽度相等，左右外边距对齐
	* text-decoration，设置文本的装饰
		* none，无任何装饰，若修饰链接，则链接的下划线会消失
		* overline，上划线
		* line-through，删除线
		* underline，下划线
	* text-transform，文本转换
		* uppercase，所有字母变为大写
		* lowercase，所有字母变为小写
		* capitalize，每个单词的首字母大写
	* text-indent，文本缩进
8. 字体
	* font-family，设置文本的字体系列，该属性应该设置几个字体名称作为一种后备机制，如果浏览器不支持第一种字体，它将尝试下一种  