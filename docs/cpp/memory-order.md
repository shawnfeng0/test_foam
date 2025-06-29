### 常量

在标头 `[<atomic>](https://zh.cppreference.com/w/cpp/header/atomic "cpp/header/atomic")` 定义

| 值 | 官方解释 | brpc的文章 | 场景理解 |
|---|---|---|---|
| 来源 | https://zh.cppreference.com/w/cpp/atomic/memory_order | https://github.com/apache/brpc/blob/master/docs/cn/atomic_instructions.md |  |
|`memory_order_relaxed`|宽松操作：没有同步或顺序制约，仅对此操作要求原子性（见下方[宽松顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E5.AE.BD.E6.9D.BE.E9.A1.BA.E5.BA.8F)）|没有fencing作用|多线程自增同一个原子变量|
|`memory_order_consume`|有此内存顺序的加载操作，在其影响的内存位置进行_消费操作_：当前线程中依赖于当前加载的该值的读或写不能被重排到此加载前。其他释放同一原子变量的线程的对数据依赖变量的写入，为当前线程所可见。在大多数平台上，这只影响到编译器优化（见下方[释放消费顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E9.87.8A.E6.94.BE.E6.B6.88.E8.B4.B9.E9.A1.BA.E5.BA.8F)）。|后面依赖此原子变量的访存指令勿重排至此条指令之前|比如读两次，可能第二次读取会被编译器优化为使用第一次读取的值，只是为了防止编译器优化。（仅对单便变量有效）|
|`memory_order_acquire`|有此内存顺序的加载操作，在其影响的内存位置进行_获得操作_：当前线程中读或写不能被重排到此加载前。其他释放同一原子变量的线程的所有写入，能为当前线程所见（见下方[释放获得顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E9.87.8A.E6.94.BE.E8.8E.B7.E5.BE.97.E9.A1.BA.E5.BA.8F)）。|后面访存指令勿重排至此条指令之前||
|`memory_order_release`|有此内存顺序的存储操作进行_释放操作_：当前线程中的读或写不能被重排到此存储后。当前线程的所有写入，可见于获得该同一原子变量的其他线程（见下方[释放获得顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E9.87.8A.E6.94.BE.E8.8E.B7.E5.BE.97.E9.A1.BA.E5.BA.8F)），并且对该原子变量的带依赖写入变得对于其他消费同一原子对象的线程可见（见下方[释放消费顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E9.87.8A.E6.94.BE.E6.B6.88.E8.B4.B9.E9.A1.BA.E5.BA.8F)）。|前面访存指令勿重排至此条指令之后。当此条指令的结果对其他线程可见后，之前的所有指令都可见||
|`memory_order_acq_rel`|带此内存顺序的读修改写操作既是_获得操作_又是_释放操作_。当前线程的读或写内存不能被重排到此存储前或后。所有释放同一原子变量的线程的写入可见于修改之前，而且修改可见于其他获得同一原子变量的线程。|acquire + release语意||
|`memory_order_seq_cst`|有此内存顺序的加载操作进行_获得操作_，存储操作进行_释放操作_，而读修改写操作进行_获得操作_和_释放操作_，再加上存在一个单独全序，其中所有线程以同一顺序观测到所有修改（见下方[序列一致顺序](https://zh.cppreference.com/w/cpp/atomic/memory_order#.E5.BA.8F.E5.88.97.E4.B8.80.E8.87.B4.E9.A1.BA.E5.BA.8F)）。|acq_rel语意外加所有使用seq_cst的指令有严格地全序关系||
