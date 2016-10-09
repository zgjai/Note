#java.util.concurrent.atomic

###atomic包简介:  
A small toolkit of classes that support _lock-free thread-safe_ programming on single variables.

1. 当在static代码块中初始化一个static final变量时，需要try catch并在catch中throw Error或者RuntimeException，不然会编译失败。

#####AtomicBoolean

1. AtomicBoolean内部实际上是通过将true和false转化为1和0去处理的，存储的实际是个int型的值。通过它的构造函数就能看出来：  


    public AtomicBoolean(boolean initialValue) {
        value = initialValue ? 1 : 0;
    }

为什么他的内部要通过int去实现，而不是通过boolean去实现呢？  
第一点，CAS操作支持的最小单位就是int；第二点，内存存储时需要对齐(不同的机器可能对齐的长度不一样，一般是8字节对齐)，所以就算用一个位去实现，实际上也并不能节省空间。
