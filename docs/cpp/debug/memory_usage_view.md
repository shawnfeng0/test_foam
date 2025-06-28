# 进程内存占用分析

参考来源：

https://stackoverflow.com/questions/56669704/clion-memory-usage-of-running-program

valgrind 的massif插件可以统计到堆内存的使用，它会生成一个massif.out.xxx的文件，里面是每次内存申请时的调用栈，可以用于分析堆内存使用是否合理。

massif-visualizer是一个用于查看massif.out.xxx文件的可视化工具。

```shell
valgrind --tool=massif XXX
massif-visualizer massif.out.xxxxx
```
