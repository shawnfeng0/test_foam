源代码中的某一个小功能或者某一个类，需要做小范围测试叫做单元测试。

我之前一直在纠结这种测试用例的代码的组织形式，目前看到有两种方案：
1. 和源代码放在同一个目录里面，比如src/fucntion1.cc src/function1_unittest.cc这种形式，可以参考google/leveldb仓库的代码。
2. 和所有的测试放在同一个目录里面，比如功能代码src/function1.cc 单元测试代码放到test/function1_unittest.cc

我一般碰到这种工程类问题就看google的代码，这次参考了<https://github.com/google/leveldb>和<https://github.com/google/mediapipe>项目，这两个项目的单元测试代码都和源码在同一个目录下。

还参考了<https://github.com/llvm/llvm-project/blob/main/libcxx/test/>llvm是把测试代码单独放在一个目录中了

使用gtest的测试用例参考：
<https://github.com/google/mediapipe/blob/v0.10.0/mediapipe/framework/deps/monotonic_clock_test.cc>
<https://github.com/google/googletest/blob/v1.13.0/googletest/test/googletest-printers-test.cc>
