# Argument Parsing

cpp命令行参数解析库：搜了一圈觉得比较好用的是：

- getopt和gengetopt工具: getopt是参数解析函数接口，gengetopt是GNU提供的快速生成getopt解析代码的辅助工具，两者配合也能达到很好的效果，并且依赖最小，且可以跨平台。
- glibc的[argp](https://www.gnu.org/software/libc/manual/html_node/Argp.html): 依赖glibc，android平台没有
- [CLI11](https://github.com/CLIUtils/CLI11): 最好用，且跨平台，仅头文件包含即可用，但头文件稍微有点大，8000+行。