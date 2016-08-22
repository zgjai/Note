#java.util.concurrent基础知识

###CAS(compare and swap)  

CAS是并发包的基石，不仅仅用于一些非阻塞的数据结构，也用于锁操作  

######CAS算法的过程：    

它包含3个参数CAS(V,E,N)，V表示要更新的变量，E表示预期值，N表示新值。只有当V＝E时，才会把V的值更新为N。如果V的值与E不同，则表示有其他线程对V作了修改，则当前线程不做任何操作。最后，CAS返回当前V的真实值。CAS操作是抱着乐观的态度进行的，当多个线程同时使用CAS操作一个变量时，只有一个会胜出，并成功更新，其余会失败，但失败的线程并不会被挂起，而仅仅是被告知失败，并可做出相应的处理。

######CPU指令：

        cmpxchg
        /*
        accumulator = AL, AX, or EAX, depending on whether a byte, word, or doubleword comparison is being performed
        */
        if(accumulator == Destination) {
            ZF = 1;
            Destination = Source;
        }else {
            ZF = 0;
            accumulator = Destination;
        }

######CAS的问题：  

_ABA问题：_  
因为CAS需要在操作值的时候检查下值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加一，那么A－B－A 就会变成1A-2B－3A。  


###volatile：  
######主内存与工作内存：  
Java内存模型规定了所有变量都存储在主内存中，每条线程还有自己的工作内存，线程的工作内存中保存了被该线程用到的变量的主内存副本拷贝，线程对变量对所有操作都必须在工作内存中进行，而不能直接读写主内存中的变量。不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递需要通过主内存来完成。一个线程对变量进行修改发生在工作内存，之后会回写到主内存，这之间的时间差可能导致数据不一致问题。通过使用volatile修饰，线程在从工作内存中读取变量时，会去读取主内存。

######CPU cache：  
cache的最小单位是cache line（缓存行），包含3个部分：  
a.2^b个字节的数据块（block），目前主流的是64字节  
b.一个有效位，标记是否有效  
c.tag，标识对应哪个内存地址  

_cache关联规则：_  
a.全关联：  
每个内存块能够被映射到任意一个cache line。操作效果上相当于一个散列表。优点在于冲突低。但是给到一个内存地址，要知道他是否存在于Cache中，需要遍历所有Cache Line并比较缓存内容的内存地址。而Cache的本意就是为了在尽可能少得CPU Cycle内取到数据。那么想要设计一个快速的全关联的Cache几乎是不可能的。  
b.直接映射：  
每个内存块只能映射到一个特定的缓存槽。优点是查询速度快。缺点是冲突严重。  
c.N路组相联：  
每个内存块能够被映射到N路特定cache line中的任意一路。比如一个16路缓存，每个内存块能够被映射到16路不同的cache line。一般地，具有一定相同低bit位地址的内存块将共享16路cache line。为什么要按低位映射？由于内存的访问通常是大片连续的，或者是因为在同一程序中而导致地址接近的（即这些内存地址的高位都是一样的）。所以如果把内存地址的高位作为set index的话，那么短时间的大量内存访问都会因为set index相同而落在同一个set index中，从而导致cache conflicts使得L2, L3 Cache的命中率低下，影响程序的整体执行效率。  
栗子：  
64位系统，4MBcache，16-way  
64位的内存地址被分为3个部分：标记，组号，偏移量
![](http://cenalulu.github.io/images/linux/cache_line/addr_bits.png)  

![](http://image24.360doc.com/DownloadImg/2011/03/0721/9778690_6.jpg)

替换策略：常用的是LRU(最近最少使用)、Random(随机)


_cache一致性协议：_  
MESI协议中的状态

CPU中每个缓存行（cache line)使用4种状态进行标记（使用额外的两位(bit)表示):

M: 被修改（Modified)

该缓存行只被缓存在该CPU的缓存中，并且是被修改过的（dirty),即与主存中的数据不一致，该缓存行中的内存需要在未来的某个时间点（允许其它CPU读取主存中相应内存之前）写回（write back）主存。当被写回主存之后，该缓存行的状态会变成独享（exclusive)状态。

E: 独享的（Exclusive)
该缓存行只被缓存在该CPU的缓存中，它是未被修改过的（clean)，与主存中数据一致。该状态可以在任何时刻当有其它CPU读取该内存时变成共享状态（shared)。同样地，当CPU修改该缓存行中内容时，该状态可以变成Modified状态。

S:共享的（Shared)

该状态意味着该缓存行可能被多个CPU缓存，并且各个缓存中的数据与主存数据一致（clean)，当有一个CPU修改该缓存行中，其它CPU中该缓存行可以被作废（变成无效状态（Invalid））。

I: 无效的（Invalid）

该缓存是无效的（可能有其它CPU修改了该缓存行）。

_cache写模式：_  
a.直写：  
透过本级缓存，直接把数据写到下一级缓存（或直接到内存）中，如果对应的段被缓存了，我们同时更新缓存中的内容（甚至直接丢弃）。  
b.回写：  
缓存不会立即把写操作传递到下一级，而是仅修改本级缓存中的数据，并且把对应的缓存段标记为“脏”段。脏段会触发回写，也就是把里面的内容写到对应的内存或下一级缓存中。回写后，脏段又变“干净”了。当一个脏段被丢弃的时候，总是先要进行一次回写。  
这就产生了一个问题，写cache跟写内存之间存在时间差，这会导致多线程数据不一致的问题吗？  
其实这个问题的关键就在一个线程在一个CPU上写时，何时通知其他CPU，如果写cache之前就通知，应该就不会产生数据不一致的问题，而当写回内存时才通知，就会导致不一致的问题。  

###class文件结构：  
每个class文件对应一个如下所示的ClassFile结构：  
u1、u2、u4分别表示1、2和4个字节的无符号数。表(table)由任意数量的可变长度的项组成，用于表示class文件内容的一系列复合结构。  
ClassFile {  
    u4          magic;          //魔数  
    u2          minor_version;  //副版本  
    u2          major_version;  //主版本  
    u2          constant_pool_count;    //  
    cp_info     constant_pool[constant_pool_count-1];   //  
    u2          access_flag;    //  
    u2          this_class;     //  
    u2          super_class;    //  
    u2          interfaces_count;   //  
    u2          interfaces[interfaces_count];   //  
    u2          fields_count;   //  
    field_info  fields[fields_count];   //  
    u2          methods_count;  //  
    method_info methods[methods_count]; //  
    u2          attributes_count;  
    attribute_info  attributes[attribute_info]; //  
}

magic是class文件的标识，固定为0xCAFEBABE。  


###对象的内存布局  
注：以下内容针对HotSpot虚拟机  
对象在内存中存储的布局可以分为3块区域：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。  
对象头
