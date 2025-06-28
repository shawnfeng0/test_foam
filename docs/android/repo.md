# repo

repo是google用来管理android的git源码仓库的工具。除了网上的正常用法，在此列出一些特殊的技巧

## 代码库过大

## 配置每个仓库仅拉取最新的commit

repo init时添加--depth参数，可以让每个仓库的代码同步都只拉取最后一次提交的代码，没有历史提交的信息，这样会大大提高拉代码的速度。

```shell
repo init --depth 1 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

初始化之后实际上是把`--depth 1`的参数存储到当前目录的.repo/manifests.git/config文件里了：

```config
[repo]
    depth = 1
```

如果是已有repo仓库，可以直接修改配置文件重新拉也行，但只会加速之后的拉取速度，之前拉过的提交会在.repo目录中继续存在。

再调用`repo sync`即可仅同步每个仓库的最新一个提交而不是下载历史提交，大大加速了仓库同步的速度。

### 恢复正常拉取所有的commit

修改.repo/manifest.git/config文件，删除`[repo] depth = 1`的配置，

### 恢复单个仓库的配置拉取所有commit

在仓库内执行git pull --unshadow命令即可拉取所有的commit历史
