##Python2.X


1. 中文编码
	在2.X版本中，若文件不指定编码，则默认使用ASCII格式；但在3.X中则默认使用UTF-8编码。所以若要在代码中包含中文，则需在文件开头加入：
		
		# -*- coding: UTF-8 -*-

2. python代码块
	python代码块不适用{}来控制类、函数等，而是使用模块缩进。
	文件格式不对时报错：IndentationError
	
3. 标准数据类型
	* Numbers （数字）
		* int （有符号整型）
		* long （长整型） 
		* float （浮点型）
		* complex （复数）
		
	* String （字符串）
	* List （列表）
	* Tuple （元组）
	* Dictionary （字典）

 
4. 算数运算符  

	|  运算符 | 描述 | 实例 |  
	| ---: | :---: | :--- |
	| + |  加  | |
	| - |  减  | |
	| * |  乘  | |
	| / |  除  | |
	| % |  取模 | |
	| ** |  幂 | 10**20表示10的20次方 |
	| // |  取整数 | 9.0//2.0输出结果为4.0 | 

5. 条件判断语句
	格式：
	if 判断条件:
		执行语句.....
	else:
		执行语句.....
	
	多判断条件的格式:
	if 判断条件1:
		执行语句1.....
	elif 判断条件2:
		执行语句2.....
	elif 判断条件3:
		执行语句3.....
	else:
		执行语句4.....
		
	示例:  
		
		flag = False
		name = 'luren'
		if name == 'python':
			flag = True
			print 'welcome boss'
		else:
			print name
			

6. while循环语句
	格式：
	while 判断条件:
			执行语句.....
	else:
			执行语句.....
			
	示例: 
	
		#!/usr/bin/python
		# -*- coding: UTF-8 -*-
		
		count=0
		while count<5:
			print count, " is less than 5"
			count = count + 1
		else:
			print count, " is not less than 5"
			

7. for循环语句
	格式1:
	for iterating_var in sequence:
		执行语句.....
		
	示例:
	
		#!/usr/bin/python
		# -*- coding: UTF-8 -*-
		
		for letter in 'Python':
			print '当前字母: ', letter 
			
	格式2:
	for index in sequence.length:
		执行语句.....
		
	示例:
	
		#!/usr/bin/python
		# -*- coding: UTF-8 -*-
		
		fruits = ['banana', 'apple', 'mango']
		for index in range(len(fruits)):
			print '当前水果: ', fruits[index]
			
8. pass语句  
	pass语句是空语句，是为了保持程序结构的完整性，一般用作占位语句。  
	

9. 数字类型转换  
	* int(x [,base ])  
		将x转换为一个整数  
	* long(x [,base ])  
		将x转换为一个长整数  
	* float(x )  
		将x转换为一个浮点数  
	* complex(real [,imag ])  
		创建一个复数  
	* str(x )  
		将对象x转换为字符串  
	* repr(x )  
		将对象x转换为表达式字符串  
	* eval(str )  
		用来计算在字符串中的有效Python表达式，并返回一个对象  
	* tuple(s )  
		将序列s转换为一个元组  
	* list(s )  
		将序列s转换为一个列表  
	* chr(x )  
		将一个整数转换为一个字符  
	* unichr(x )  
		将一个整数转换为Unicode字符  
	* ord(x )  
		将一个字符转换为它的整数值  
	* hex(x )  
		将一个整数转换为一个十六进制字符串  
	* oct(x )  
		将一个整数转换为一个八进制字符串  
		
10. 数学函数
	* abs(x)  
		返回数字的绝对值
	* ceil(x)  
		返回数字的上入整数，如math.ceil(4.1)返回5  
	* cmp(x,y)  
		若x\<y返回-1，若x==y返回0，若x>y返回1  
	* exp(x)  
		返回e的x次幂(e<sup>x</sup>)，如math.exp(1)返回值为2.718281828459045  
	* fabs(x)  
		返回数字的绝对值的浮点值，math.fabs(-10)返回10.0  
	* floor(x)  
		返回数字的下舍整数  
	* log(x)  
		如math.log(math.e)返回1.0，math.log(100,10)返回2.0  
	* log10(x)  
		返回以10为基数的x的对数，如math.log10(100)返回2.0  
	* max(x1,x2...)   
		返回给定参数的最大值，参数可以为序列  
	* min(x1,x2...)  
		返回给定参数的最小值，参数可以为序列  
	* modf(x)  
		返回x的整数部分与小数部分，两个部分的数值符号与x相同，整数部分以浮点型表示  
	* pow(x,y)  
		x<sup>y</sup> 运算后的值  
	* round(x [,n])  
		返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数    
	* sqrt(x)  
		返回数字x的平方根
		
11. 随机数函数
	* choice(seq)  
		从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数  
	* randrange([start,] stop [,step])  
		从指定范围内，按指定基数递增的集合中获取一个随机数，基数缺省值为1  
	* random()  
		随机生成下一个实数，它在[0,1)范围内  
	* seed([x])  
		改变随机数生成器的种子seed  
	* shuffle(lst)  
		将序列的所有元素随机排序  
	* uniform(x,y)  
		随机生成下一个实数，它在[x,y]范围内

12. 字符串
	Python不支持单字符类型，单字符也是作为一个字符串使用。Python访问字符串，可以使用方括号来截取字符串。  
	示例：
	
		#!/usr/bin/python
		# -*- coding: utf-8 -*-
		
		var1 = 'Hello World!'
		var2 = "Python Runoob"
		
		print "var1[0]: ", var1[0]
		print "var2[1:5]: ", var2[1:5]
		
	示例执行结果为：  
	
		var1[0]: H
		var2[1:5]: ytho
		
	
13. 字符串运算符  
	假设a="Hello", b="Python"
	
	| 操作符 | 描述 | 结果 |
	|---|:-----|:-----|
	| + | 字符串 | HelloPython |
	| * | 重复输出字符串 | a*2结果为HelloHello |
	| [] | 通过索引获取字符串中字符 | a[1]结果为e |
	| [:] | 截取字符串中的一部分 | a[1:4]结果为ell |
	| in | 成员运算符-如果字符串中包含给定的字符返回True | H in a结果为1 |
	| not in | 成员运算符-如果字符串中不包含给定的字符返回True | M not in a 结果1 |
	| r/R | 原始字符串 - 原始字符串：所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符。 原始字符串除在字符串的第一个引号前加上字母"r"（可以大小写）以外，与普通字符串有着几乎完全相同的语法。 | print r'\n' 输出 \n 和 print R'\n' 输出 \n |
	| % | 格式字符串 | 类似于c中的sprintf |  
	
14. 三引号  
	Python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其它特殊字符，自始至终保持一小块字符串的格式是所谓的WYSIWYG（所见即所得）格式的。
	
	
15. Python列表(List)  
	列表是最常用的Python数据类型，它可以作为一个方括号内的逗号分隔值出现。列表的数据项不需要具有相同的类型。与字符串的索引一样，列表索引从0开始，列表可以进行截取、组合等。
	示例：
	
		list1 = ['physics', 'chemistry', 1997, 2000];
		list2 = [1, 2, 3, 4, 5];
		list3 = ["a", "b", "c", "d"];
		
	* 删除列表元素  
		del list1[2]：表示删除list1的第3个元素  
	* 列表脚本操作符  
		示例：  
		
		| Python表达式 | 结果 | 描述 |
		|:-----|:-----|:-----|
		| len([1,2,3]) | 3 | 长度 |
		| [1,2,3] + [4,5,6] | [1,2,3,4,5,6] | 组合 |
		| ['Hi!']*4 | ['Hi!','Hi!','Hi!','Hi!'] | 重复 |
		| 3 in [1,2,3] | True | 元素是否存在于列表中 |
		| for x in [1,2,3]: print x | 1 2 3 | 迭代 |   
	
	* 列表截取  
		示例：L = ['spam', 'Spam', 'SPAM!']
		
		| Python表达式 | 结果 | 描述 |
		|:-----|:-----|:-----|
		| L[2] | 'SPAM!' | 读取列表中第三个元素 |
		| L[-2] | 'Spam' | 读取列表中倒数第二个元素 |
		| L[1:] | ['Spam', 'SPAM!'] | 从第二个元素开始截取列表 |
		
16. Python元组(tup)  
		Python元组与列表类似，不同之处在于元组的元素不能修改。元组使用小括号，列表使用方括号。
		注意：元组中只包含一个元素时，需要在元素后面添加逗号，如：  tup = (50,);  
		
17. Python字典(Dictionary)
		字典是另一种可变容器模型，且可存储任意类型对象。  
		示例：  
		
		dict = { 'abc':123, 98.6:37 };
		#获取字典里的值  
		print dict['abc'], dict[98.6];
		#修改字典里的值
		dict['abc'] = 8;
		#删除字典元素
		del dict['abc'];
		#清空字典元素
		dict.clear();
		#删除字典
		del dict;		
		
18. Python日期和时间  
		Python提供了一个time和calendar模块可以用于格式化和时间。时间间隔是以秒为单位的浮点小数。
		示例：
		
		#!/usr/bin/python
		# -*- coding: UTF-8 -*-
		
		import time;  # 引入time模块
		
		ticks = time.time()
		print "当前时间戳为：", ticks
		
		
	时间元组：
		
	| 序号 | 字段 | 值 |
	|:-----|:-----|:-----|
	| 0 | 4位数年 | 2008 |
	| 1 | 月 | 1到12 |
	| 2 | 日 | 1到31 |
	| 3 | 小时 | 0到23 |
	| 4 | 分钟 | 0到59 |
	| 5 | 秒 | 0到61（60或61是闰秒）|
	| 6 | 一周的第几日 | 0到6（0是周一） |
	| 7 | 一年的第几日 | 1到366（儒略历） |
	| 8 | 夏令时 | -1,0,1,-1是决定是否为夏令时的旗帜 |
		