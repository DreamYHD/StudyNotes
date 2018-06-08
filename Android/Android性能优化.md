#### 布局优化
在既可以使用linearlayout和relativelayout的时候使用linearlayout，如果布局优点复杂，简单的linearlayout不能满足的时候，使用过relativelayout，尽量多实用include，merger标签，还有可以延迟加载的场景使用viewstub；
#### 绘制优化1
在draw方法中，尽量不要新建变量，防止oom，
#### 响应速度优化
主要真的anr出现，activity5 秒钟，broadcastreceiver10秒，service20秒。可以创建线程或者使用acynctask，或者intentService或者handlerThread来完成一些耗时操作。
#### 内存优化
内存溢出
内存泄漏
#### 线程优化
使用线程池


