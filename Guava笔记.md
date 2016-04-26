#Guava wiki阅读翻译笔记

1. 使用和避免使用null
	轻率地使用null可能会导致很多让人惊愕的bug。另外，null的含义是模糊的。例如，Map.get(key)返回null时，这有可能表示这个键对应的值为null，也可能表示该Map中不存在该key。null可以表示失败、成功或者任何情况。使用null以外的特定值，会让你表达的含义跟明确。  
	当然，null也有一些合适的使用场景。在性能和速度方面，null是很有优势的，并且，在对象数组中，null是不可避免的。但是在应用代码中，尤其是对于底层库来说，null是导致各种混乱和疑难问题的元凶。最关键的是，null本身没有定义它表达的意思。  
	因为这些原因，guava的很多工具类对于null都采用了快速失败的操作，除非工具类本身有针对null的处理措施。
	
	#####Optional
	很多情况下，开发人员用null来表示某种缺失情形：可能已经有一个默认值，或没有值，或找不到值。  
	Guava用Optional<T>来表示一个可能为null的的T类型引用，一个Optional实例可能包含一个非null的引用（称之为引用存在），也可能什么都不包含（称之为引用缺失）。  
	示例：
	
		Optional<Integer> possible = Optional.of(5);
		possible.isPresent();  //return true
		possible.get();        //return 5
		
	