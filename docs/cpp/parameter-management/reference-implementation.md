# Reference Implementation

## 参数管理的参考实现

### 鸿蒙系

开源的openharmony分布式数据管理模块有两个可以参考的参数管理实现

源码地址：<https://gitee.com/openharmony/distributeddatamgr_appdatamgr/blob/master/README_zh.md>

> 关系型数据库（Relational Database，RDB）
>
> 是一种基于关系模型来管理数据的数据库。OpenHarmony关系型数据库基于SQLite组件提供了一套完整的对本地数据库进行管理的机制。
>
> 首选项（Preferences）
>
> 主要提供轻量级Key-Value操作，支持本地应用存储少量数据，数据存储在本地文件中，同时也加载在内存中，所以访问速度更快，效率更高。首选项提供非关系型数据存储，不宜存储大量数据，经常用于操作键值对形式数据的场景。
>
> 数据共享（DataShare）
>
> 主要用于应用管理其自身数据，同时支持同个设备上不同应用间的数据共享。
>
> 轻量系统KV数据库（Lightweight KV store）
>
> 依托当前公共基础库提供的KV存储能力开发，为轻量系统设备应用提供键值对数据管理能力。在有进程的平台上，KV存储提供的参数管理，供单进程访问不能被其他进程使用。在此类平台上，KV存储作为基础库加载在应用进程，以保障不被其他进程访问。

其中首选项和轻量系统KV数据库是可以参考的。

#### open鸿蒙的首选项(preference)属性读写实现

源码地址: <https://gitee.com/openharmony/distributeddatamgr_preferences>

看了下源码，文件储存结构是使用的xml结构读写，读出来后在内存里面维护一个map，写的时候直接修改内存里面的map，然后再写回xml文件。

#### open鸿蒙的轻量系统KV数据库(lightweight kv store)属性读写实现

源码地址: <https://gitee.com/openharmony/distributeddatamgr_kv_store>

这个实现是把每一个key当作一个文件名，value当作文件内容，然后把这些文件放在一个文件夹里面，实现了简单的kv数据库。

在网上找到了一个类似的简单实现：<https://yoursunny.com/t/2021/openat/> 也是key做文件名，value做文件内容，然后放在一个文件夹里面。

### Android property service

Android property service是Android系统的一个属性管理服务，可以用来管理系统属性，也可以用来管理应用属性。

源码地址：<https://github.com/aosp-mirror/platform_system_core/blob/master/init/property_service.cpp>

参数都储存在一个单文件中，内部结构是一个字典树。查找效率也很高，值得移植出来做适配。

property的参数名和参数值都有长度限制：PROPERTY_KEY_MAX: 32 PROPERTY_VALUE_MAX: 92
可以在android的源码中找到: include/cutils/properties.h，不同的android的版本放置位置可能不同。

参数获取的接口可以参考一下：
旧版本C函数接口：<https://android.googlesource.com/platform/system/core/+/refs/tags/android-platform-13.0.0_r5/libcutils/include/cutils/properties.h>
新版本C++函数接口：<https://android.googlesource.com/platform/system/libbase/+/refs/tags/android-platform-13.0.0_r5/include/android-base/properties.h>

android/安卓源码查看工具：&lt;cs.android.com>

### ros

使用yaml格式读取和存储参数，运行时只在内存里面保存参数。

命令行工具的使用参数可以参考一下：<https://docs.ros.org/en/humble/How-To-Guides/Using-ros2-param.html>

### lmdb

参考：[Lmdb](/database/lmdb.md)