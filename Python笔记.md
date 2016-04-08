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
	
