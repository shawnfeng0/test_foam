# File_monitor

## 如何监控文件修改和创建

### inotify

linux有一个机制inotify可以监控文件系统inode节点的文件操作，有个inotify-tools工具可以方便的使用这个机制。

可以监控到哪个目录/文件被创建/修改/删除了，但是无法统计到操作的用户名，和进程id。

inotify-tools无法获取文件修改的源pid，相关issue：<https://github.com/inotify-tools/inotify-tools/issues/89>

### auditctl

auditctl是一个审计工具，它需要对内核做一些修改，然后应用层调用对应达到监控的目的，它是可以获取到修改文件的进程id的。可以更好的监控。

源码位置：<https://github.com/linux-audit>