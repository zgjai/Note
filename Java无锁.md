#无锁
1.CAS  
CAS算法的过程：  
它包含3个参数CAS(V,E,N)，V表示要更新的变量，E表示预期值，N表示新值。只有当V＝E时，才会把V的值更新为N。如果V的值与E不同，则表示有其他线程对V作了修改，则当前线程不做任何操作。最后，CAS返回当前V的真实值。CAS操作是抱着乐观的态度进行的，当多个线程同时使用CAS操作一个变量时，只有一个会胜出，并成功更新，其余会失败，但失败的线程并不会被挂起，而仅仅是被告知失败，并可做出相应的处理。

CPU指令：

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
