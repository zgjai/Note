# channel源码分析

## 数据结构
**整体结构**

    type hchan struct {
    	qcount   uint           // total data in the queue  缓冲槽有效数据项数量
    	dataqsiz uint           // size of the circular queue  缓冲槽大小(可存储数据项数量)
    	buf      unsafe.Pointer // points to an array of dataqsiz elements  缓冲槽指针
    	elemsize uint16 // 数据项大小
    	closed   uint32	// 是否关闭
    	elemtype *_type // element type 数据项类型
    	sendx    uint   // send index 缓冲槽发送位置索引
    	recvx    uint   // receive index 缓冲槽接收位置索引
    	recvq    waitq  // list of recv waiters 接收者等待队列
    	sendq    waitq  // list of send waiters 发送者等待队列
    
    	// lock protects all fields in hchan, as well as several
    	// fields in sudogs blocked on this channel.
    	//
    	// Do not change another G's status while holding this lock
    	// (in particular, do not ready a G), as this can deadlock
    	// with stack shrinking.
    	lock mutex
    }
    
    
**_type结构**
    
    // Needs to be in sync with ../cmd/compile/internal/ld/decodesym.go:/^func.commonsize,
    // ../cmd/compile/internal/gc/reflect.go:/^func.dcommontype and
    // ../reflect/type.go:/^type.rtype.
    type _type struct {
    	size       uintptr
    	ptrdata    uintptr // size of memory prefix holding all pointers
    	hash       uint32
    	tflag      tflag
    	align      uint8
    	fieldalign uint8
    	kind       uint8
    	alg        *typeAlg
    	// gcdata stores the GC type data for the garbage collector.
    	// If the KindGCProg bit is set in kind, gcdata is a GC program.
    	// Otherwise it is a ptrmask bitmap. See mbitmap.go for details.
    	gcdata    *byte
    	str       nameOff
    	ptrToThis typeOff
    }


**waitq结构**

    type waitq struct {
	   first *sudog
	   last  *sudog
    }

**sudog结构**

    // sudog represents a g in a wait list, such as for sending/receiving
    // on a channel.
    //
    // sudog is necessary because the g ↔ synchronization object relation
    // is many-to-many. A g can be on many wait lists, so there may be
    // many sudogs for one g; and many gs may be waiting on the same
    // synchronization object, so there may be many sudogs for one object.
    //
    // sudogs are allocated from a special pool. Use acquireSudog and
    // releaseSudog to allocate and free them.
    type sudog struct {
    	// The following fields are protected by the hchan.lock of the
    	// channel this sudog is blocking on. shrinkstack depends on
    	// this.
    
    	g          *g
    	selectdone *uint32 // CAS to 1 to win select race (may point to stack)
    	next       *sudog
    	prev       *sudog
    	elem       unsafe.Pointer // data element (may point to stack)
    
    	// The following fields are never accessed concurrently.
    	// waitlink is only accessed by g.
    
    	acquiretime int64
    	releasetime int64
    	ticket      uint32
    	waitlink    *sudog // g.waiting list
    	c           *hchan // channel
    }


