# 移植libarchive到android的AOSP中

https://github.com/libarchive/libarchive

我是2023-06-12号移植的master分支的最新的提交，当时最新的release是3.6.2

## 移植到aosp中

首先看到libarchive/libarchive/blob/master/contrib/android/Android.mk目录中包含了一个android.mk，但是这个编译脚本很旧，把源码拷贝到aosp中后，开始了移植过程。

1. 首先在外层引用这个android.mk, 使用如下类似的语法

```makefile
include $(LIB_MY_DIR)/libarchive-master/contrib/android/Android.mk
```

2. 发现在contrib/android/Android.mk里面有两份libarchive的编译描述块，而且定义的-DPLATFORM_CONFIG_H定义的配置头文件不是同一个，删除了不是android.h的那一份。

3. 在android.h的配置文件中贴 `#define __LIBARCHIVE_CONFIG_H_INCLUDED 1`，不然会编译不过，应该是之后新增的，android.h没有更新

4. `libarchive/archive_entry_copy_stat.c:48-50`行里面st->st_atime_nsec，需要把最后的下划线去掉改成st_atimensec，可能是之前手误写错了的，这个可以参考https://github.com/freebsd/freebsd-src/blob/master/sys/sys/stat.h，在网上搜了一下st_atime_nsec，好像rust语言的库里面是用的这个名字，但是libc库里面的不是。

5. 根据编译报错的未定义的符号的错误，找到需要编译的文件，添加到android.mk里面，缺啥补啥，注意不要重复添加同一个文件两份，不然会导致符号多重定义也会报错。

6. 如果没有liblz4的话，可以把libarchive编译描述里面的liblz4的依赖项去掉。