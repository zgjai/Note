#第二章 URL与资源
1. URL语法通用格式  
	大多数URL方案的URL语法都建立在这个由9部分构成的通用格式上：  

		<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>
		
	URL最重要的3个部分是方案（scheme）、主机（host）和路径（path）。  
	
	|   组件 |  描述   |   默认值  |
	| ----  |:-----: | ---------:|
	|  方案	  |	 访问服务器以获取资源时要使用哪种协议   |  无   |
	