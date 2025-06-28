# mac首次配置

mac默认使用zsh作为默认的shell

首先先安装oh-my-zsh。

## 解决科学上网

### 临时方案

临时方案就是使用先使用其他设备的代理，把依赖科学上网才能安装的软件安装完。

前置操作：

1. 先让手机开启代理，并且启用局域网代理
2. mac和手机连接同样的wifi 获取手机的ip地址，假设是192.168.3.34

#### 终端中科学上网

把以下命令写入到.zshrc中，执行proxy开启代理，unproxy关闭代理

```Shell
alias proxy='export all_proxy=socks5://192.168.3.34:10808'
alias unproxy='unset all_proxy'
```

#### 系统全局代理配置

系统设置中搜索代理，配置代理方法、IP、端口即可

### mac安装xray自主代理

安装brew: 终端开代理后直接执行官方安装脚本安装

安装xray:
1. 安装xray：brew install xray
2. 把手机上的配置导出来，写入到mac的xray配置文件中：vim /opt/homebrew/etc/xray/config.json
3. 设置开机自启：brew services start xray
4. 修改后重启xray服务：brew services restart xray

此时可以把上面的系统配置代理和终端配置的代理的ip地址修改成127.0.0.1了，测试是否可用。

`curl ipinfo.io`终端中执行此命令可以看到ip地址的物理归属地。

## 安装zsh的其他效率插件

`brew install zsh-syntax-highlighting`安装完成后在.zshrc中添加脚本开启：

`[ -f /opt/homebrew/etc/profile.d/autojump.sh ] && . /opt/homebrew/etc/profile.d/autojump.sh`

`brew install zsh-syntax-highlighting`安装后添加以下脚本到.zshrc

`source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh`

