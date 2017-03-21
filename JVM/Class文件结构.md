# Class文件结构

目前活跃在JVM上的语言，除了Java，还有Scala、Groovy、Clojure等等。对于JVM来说，Class文件是其对外的一个接口。只要Class文件的结构符合标准，不管编写这个程序的源语言是什么，JVM都能够执行它。详细的Class文件结构描述，可以查看相应版本的Java虚拟机规范，本系列文章主要结合实例探索Class文件的主要组成部分。

### Class文件整体结构
Java8虚拟机规范中对Class文件结构的定义如下：

    ClassFile {       u4             magic;       u2             minor_version;       u2             major_version;       u2             constant_pool_count;       cp_info        constant_pool[constant_pool_count-1];       u2             access_flags;       u2             this_class;       u2             super_class;       u2             interfaces_count;       u2             interfaces[interfaces_count];       u2             fields_count;       field_info     fields[fields_count];       u2             methods_count;       method_info    methods[methods_count];       u2             attributes_count;       attribute_info attributes[attributes_count];    }

所有的数据类型都是无符号数(u)，数字表示所占用的字节数(4表示4字节，2表示2字节)。以_info结尾的是结构体，其内部实际上还是由一个个的无符号数构成。

先介绍一下本系列查看Class文件内容结构的两个工具：
**a.Hex Fiend**
作用为查看一个Class文件的16进制源码，效果如([图1](#1))所示

<h5 id="1">图1</h5>
![](http://o793hh4oz.bkt.clouddn.com/14764462536409.jpg)

**b.jclasslib**
作用为可视化的查看Class文件的内容结构，效果如([图2](#2))所示

<h5 id="2">图2</h5>
![](http://o793hh4oz.bkt.clouddn.com/14764470928090.jpg)

<h5 id="1-1">图1-1</h5>
![](http://o793hh4oz.bkt.clouddn.com/14764376829625.jpg)

***magic(魔数)***
用于标识Class文件，固定为：0xCAFEBABE。[图1-1](#1-1)是一个class文件的开头一部分，可以看到文件的开始就是magic，占用4字节。

***minor_version(次版本号)*** && ***major_version(主版本号)***
这两个版本号共同构成了class文件的版本，这个版本的作用在于，让虚拟机知道，当前class文件是不是可被执行。基本规则是，高版本的虚拟机可以执行低版本(<=关系)的class文件，低版本的虚拟机不能执行高版本(>关系)的class文件。
如[上图1-1](#1-1)，可以看出这个class文件的主版本为52(十进制)，次版本为0。由1.8版本的javac编译器编译生成的class文件的版本号就是这样的，而这个class就可以被1.8的虚拟机执行。

***constant_pool_count(常量池数量)***
记录了这个Class中的所有常量的数量加1，也即常量池的大小。
如上图1-1，这个class的常量池数量的大小为129。

***constant_pool(常量池)***
常量池中存储着这个class相关的所有常量，它的大小是所有常量的数量加1，因为index为0时，内容为空。
> 在Class文件格式规范制定之时，设计者将第0项常量空出来是有特殊考虑的，这样做的目的在于满足后面某些指向常量池的索引值的数据在特定情况下需要表达“不引用任何一个常量池项目”的含义，这种情况就可以把索引值置为0来表示。(该段话引用自周志明老师的《深入理解Java虚拟机》第2版)

常量池中存储的数据类型有多种，但它们又一个共同的抽象结构：

    cp_info {
        u1 tag;
        u1 info[];
    }
其中tag用于标识是哪种类型的常量，具体的对应关系表见[官方文档](http://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.4-140)。info[]字段的内容格式根据tag的值的变化而变化。具体的每种常量类型的介绍在本系列后续进行。

### 未完待续。。。


### 续
***access_flags(访问标识)***
访问标识用于表示Class的属性(final、public等，具体值与描述见[官方文档](http://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.1-200-E.1))。

***this_class(类索引)***
当前Class的名称在常量池中的索引。

***super_class(父类索引)***
当前Class的父类的名称在常量池中的索引。

***interfaces_count(接口数量)***
当前Class继承的接口数量。

***interfaces[interfaces_count]***
当前Class继承的接口的名称在常量池中的索引组成的列表。

***fields_count(变量数量)***
当前Class包含的变量数量。

***fields[fields_count]***
fields表中存储的数据都是具有相同结构的：

    field_info {
        u2             access_flags;
        u2             name_index;
        u2             descriptor_index;
        u2             attributes_count;
        attribute_info attributes[attributes_count];
    }
access_flags的值与前面Class的access_flags的值有一些区别，[详情](http://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.5-200-A.1)。

name_index是这个字段的名称在常量池中的索引位置。

descriptor_index这个后续结合实例细看。

attributes_count是附加属性的数量。

attributes[attributes_count]自然就是附加属性表。


***methods_count(方法数量)***
当前Class包含的方法数量。

***methods[methods_count]***
methods表中存储的数据的结构为:

    method_info {
        u2             access_flags;
        u2             name_index;
        u2             descriptor_index;
        u2             attributes_count;
        attribute_info attributes[attributes_count];
    }

***attributes_count(附加属性数量)***
当前的Class的附加属性的数量。

***attributes[attributes_count]***
attributes表中存储的数据的结构为:

    attribute_info {
        u2 attribute_name_index;
        u4 attribute_length;
        u1 info[attribute_length];
    }


#### 以上两篇的内容就是Class文件的整体结构，从第三篇开始将结合实例分析Class文件的结构。


### 未完待续。。。


### 续
接下来，通过两个Class文件分析JVM规定的Class的文件结构，分析以jclasslib为主，Hex为辅。两个Class的源码如下：

    public interface ClassInterface {

        void testInterface();

        int returnInt();

        int num = 1;

        String str = "str";
    }


```
public class ClassStructureTest implements ClassInterface {
    private static final String privateFinalStaticString = "privateFinalStaticStringValue";
    private static String privateStaticString = "privateStaticStringValue";
    private final String privateFinalString = "privateFinalStringValue";
    public String publicString = "publicStringValue";
    private String privateString = "privateStringValue";
    public Integer publicInteger = 0x33;
    private int privateInt = 0x44;
    private final Long privateFinalLong = 0x1111L;
    public final long publicFinalNativeLong = 0x2222L;
    private static final List<String> privateStaticFinalStringList = new ArrayList<String>();
    public static Map<String, String> publicStaticMap = new HashMap<String, String>();
    public static final int num = 2;

    static {
        privateStaticFinalStringList.add("first");
        privateStaticFinalStringList.add("second");
        privateStaticFinalStringList.add("third");

        publicStaticMap.put("1", "firstValue");
        publicStaticMap.put("2", "secondValue");
    }

    public static void staticFunction() {
        System.out.print("staticFunction");
    }

    public void function() {
        System.out.print("function");
    }

    public void testInterface() {

    }

    public int returnInt() {
        return 0;
    }

}
```

可以看到第一个是一个接口，第二个是一个继承了这个接口的类。[图2-1](#2-1)是ClassInterface的Class文件结构，[图2-2](#2-2)是ClassStructureTest的Class文件结构。[图2-3](#2-3)是Object的Class文件结构。

<h5 id="2-1">图2-1</h5>
![](http://o793hh4oz.bkt.clouddn.com/14772743125046.jpg)

<h5 id="2-2">图2-2</h5>
![](http://o793hh4oz.bkt.clouddn.com/14772743229192.jpg)

<h5 id="2-3">图2-3</h5>
![](http://o793hh4oz.bkt.clouddn.com/14773570626501.jpg)



从上面3张图中，可以清晰的看出JVM规范中定义的Class的文件结构，其中magic字段没有写出，因为每个Class文件的magic字段都一样，为一个定值(0xCAFEBABE)。
其他的字段都是按照规范中定义的顺序依次列出的，其中5个数组结构的对应着图中左侧5个折叠的文件夹，需要注意的是，但某个数组为空时，可以看到其样式不再是文件夹，而是普通的文件。
比如[图2-1](#2-1)中  **Interfaces count**  为0，相应的 **Interfaces[]** 数组就为空。而在实际存储时，如果数组为空，则不占用任何字节。如[图1-2](#1-2)，这是ClassInterface类的Class文件由Hex打开的一部分内容，截图内容从 **Access flags** 开始，为0x0601，跟[图2-1](#2-1)中是相符合的，接下去是 **This class** 为0x0001， **Super class** 为0x0002。再接下去，就应该是 **Interfaces count** 的值，为0x0000，即为0，这时 **Interfaces[]** 为空，不占用任何字节，从 [图1-2](#1-2) 中可以看到在0x0000之后是0x0002，这个其实是 **Fields count** 的值。 

<h5 id="1-2">图1-2</h5>
![](http://o793hh4oz.bkt.clouddn.com/14773585213202.jpg)


下面结合3张图，看一下这3个类的Class文件属性都有什么异同。

* 由于3个Class文件都是由1.8版本的javac编译而成，所以它们的 **Minor version** 和 **Major version** 都相同。
* **Constant pool count** 即 **常量池大小** 完全跟类的具体内容相关，所以3个Class文件的该值均不同是完全正常的（当然了，这个值也是可能一样的）。
* **Access flags** 表示类的访问属性，可以看到两个public class (*Object, ClassStructureTest*) 的属性值都为0x0021，这是由两个flag (*ACC_PUBLIC, ACC_SUPER*) 构成，其中 *ACC_SUPER* 是现代编译器编译class和enum时都会设置的(interface和annotation不会)。再来看ClassInterface的访问属性，为0x0601 (*ACC_PUBLIC, ACC_INTERFACE, ACC_ABSTRACT*)，public,interface都很好理解，而接口自然是抽象的，没有具体实现的，所以也设置了abstract属性。一个public enum的访问属性值为 0x4031， 一个public annotation的访问属性值为 0x2601，可以自己思考一下为什么。
* **This class | Super class** 是当前类或者父类的信息在常量池中的索引，他们在常量池中的存储结构都是一样的，具体的结构后续讲。需要注意的是Object的父类，我们都知道java中Object是终极父类，它自己是没有父类的。这时候前文讲到的 **常量池中存储着这个class相关的所有常量，它的大小是所有常量的数量加1，因为index为0时，内容为空。** 就派上用场了，将Object的父类的索引指向常量池中不存在的0就可以了，表示不存在。
* 下面几个count都是类中实际信息得出的值，没什么好说的，有兴趣的可以自己数一下。


#### 下一篇将会展开看几个池的存储内容和结构

### 未完待续。。。


### 续

最近在看《自己动手写Java虚拟机》，作者也写了一个查看Class文件结构的工具，叫classpy，可以直接在github上下到jar包。

<h5 id="3-1">图3-1</h5>
![](http://o793hh4oz.bkt.clouddn.com/14785682357420.jpg)

[图3-1](#3-1)就是该软件的截图。

接下来，详细了解一下常量池(**Constant pool**)的存储结构。常量池中的内容都符合这样的格式:

    cp_info {        u1 tag;        u1 info[]; 
    }

其中tag的作用就是标识 **info[]** 应该是什么样的内容，具体的不同类型与tag的对应关系可以参见[点我](http://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.4-140)。表格中第一列就是存储的数据的类型，第二列就是相应的tag值。

我们按 *tag值从小到大的顺序* 来了解一下每种类型的结构。

#### CONSTANT_Utf8_info
该类型结构为

    CONSTANT_Utf8_info {        u1 tag;        u2 length;        u1 bytes[length];    }

<h5 id="3-2">图3-2</h5>
![](http://o793hh4oz.bkt.clouddn.com/14787405615222.jpg)
[图3-2](#3-2)是常量池中索引为70的位置存储的数据，类型是Utf8。

* tag  
    此处应为1，[图3-2](#3-2)中tag符合。
* length  
    bytes[]数据长度，因为该字段占用2个字节，最大为65535，也就意味着代码中方法变量命名，最大不能超过64KB(因为可以为中文，中文占用的大小跟英文不一样，只能通过占用空间大小判断)。[图3-2](#3-2)中存储的数据长度为7，可以通过存储的内容 *zeroInt* 验证。
* bytes[]  
    bytes[]中存储着采用了 [**Modified UTF-8**](http://docs.oracle.com/javase/7/docs/api/java/io/DataInput.html#modified-utf-8) 编码的数据。[图3-2](#3-2)中即为 *zeroInt* ，这实际上是在java文件中定义的一个变量的名称。
    
    
#### CONSTANT_Integer_info
该结构类型为

    CONSTANT_Integer_info {        u1 tag;        u4 bytes; 
    }

<h5 id="3-3">图3-3</h5>
![](http://o793hh4oz.bkt.clouddn.com/14787415639542.jpg)
[图3-3](#3-3)是常量池中索引为69的位置存储的数据，类型是Integer。

* tag  
    此处应为3
* bytes  
    存储着int常量的值，占用4个字节，按大端字节序存储。
    
#### CONSTANT_Float_info
该类型其实跟 **CONSTANT_Integer_info** 结构完全一样，区别在于bytes中存储的是符合 **IEEE 754** 标准的浮点数，tag值为4。

#### CONSTANT_Long_info
该类型结构为

    CONSTANT_Long_info {       u1 tag;       u4 high_bytes;       u4 low_bytes;    }

<h5 id="3-4">图3-4</h5>
![](http://o793hh4oz.bkt.clouddn.com/14787431628554.jpg)
[图3-4](#3-4)是常量池中索引为11的位置存储的数据，类型是Long。
**注意**
在Class文件中，所有占用8字节的常量(Long、Double)，都会被分为两部分，在常量池中也会占用两个位置，如[图3-4](#3-4)中的Long常量，就占用了索引位置为11和12的两个位置。下一个常量池中的数据的索引就是13。

* tag  
    此处应为5
* high_bytes  
    Long型常量的高4字节的数据
* low_bytes  
    Long型常量的低4字节的数据

#### CONSTANT_Double_info
该类型其实跟 **CONSTANT_Long_info** 结构完全一样，区别在于bytes中存储的是符合 **IEEE 754** 标准的浮点数，tag值为6。


### 未完待续。。。


### 续

#### CONSTANT_Class_info
该类型结构为

    CONSTANT_Class_info {       u1 tag;       u2 name_index;    }

Class类型是用来表示一个Class或者一个interface的。  
<h5 id="3-5">图3-5</h5>
![](http://o793hh4oz.bkt.clouddn.com/14790861453721.jpg)
[图3-5](#3-5)是常量池中索引为27的位置存储的数据，类型为Class。它存储的内容中包含了一个指向常量池中索引为112位置的数据，见[图3-6](#3-6)，它的类型是UTF8，存储着Class类型对应的Class的包路径，即ArrayList的路径。

<h5 id="3-6">图3-6</h5>
![](http://o793hh4oz.bkt.clouddn.com/14790861605695.jpg)

* tag
    此处应为7
* nameIndex
    指向常量池的索引，被指向的位置应当有效，且数据类型为 **CONSTANT_Utf8_info**
    
#### CONSTANT_String_info
该类型结构为

    CONSTANT_String_info {       u1 tag;       u2 string_index;    }
    
String类型是用于表示String类型的常量。  
* tag
    此处应为8
* nameIndex
    指向常量池的索引，被指向的位置应当有效，且数据类型为 **CONSTANT_Utf8_info**，其实跟Class类型是一样的

#### CONSTANT_Fieldref_info 
#### CONSTANT_Methodref_info
#### CONSTANT_InterfaceMethodref_info
这3种类型的结构相似

    CONSTANT_Fieldref_info {        u1 tag;        u2 class_index;        u2 name_and_type_index;    }
    
    CONSTANT_Methodref_info {        u1 tag;        u2 class_index;        u2 name_and_type_index;    }
        CONSTANT_InterfaceMethodref_info {        u1 tag;        u2 class_index;        u2 name_and_type_index;    }

分别用来表示变量、方法和接口方法。

* tag  
    依次为9、10、11
* class_index  
    指向常量池的索引，被指向位置应当有效，且数据类型为 **CONSTANT_Class_info**
* name_and_type_index  
    指向常量池的索引，被指向位置应当有效，且数据类型为**CONSTANT_NameAndType_info**

