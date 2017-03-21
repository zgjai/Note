## func Compare

    func Compare(a, b string) int
    返回值：a==b时为0，a>b时为1，a<b时为-1

Compare函数用于比较两个字符串，**但是，强烈建议不要使用，而应该直接使用==、<、>**，代码中注释如下：

> Compare is included only for symmetry with package bytes.It is usually clearer and always faster to use the built-in string comparison operators ==, <, >, and so on.


## func Contains

    func Contains(s, substr string) bool
    返回值：s中包含substr为true，否则为false
    
**实现原理：**
直接通过另一个函数Index(s, sep string)来实现，但Index的返回值大于等于0时，就表示存在这个substr。

## func ContainsAny

