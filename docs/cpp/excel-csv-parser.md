# Excel Csv Parser

## excel文件读取

excel文件的格式较复杂，建议在电脑上转换成csv格式的之后使用csv解析库解析

## csv文件读取

csv格式算是简化版的excel，行之间使用'\\n'分割，列使用','分割

在github上找到两个csv格式的C++解析库：

<https://github.com/ben-strasser/fast-cpp-csv-parser>：这个star数量1.8k+，但是无法解析字符串里面包含'\\n'的场景，它会当作一个新行处理。

<https://github.com/vincentlaucsb/csv-parser>：这个库可以很好的兼容带有'\\n'的字符串。并且生成可以兼容标准(RFC 4180)对csv的定义。

<https://stackoverflow.com/questions/1120140/how-can-i-read-and-parse-csv-files-in-c>: stackoverflow上面的简单实现。