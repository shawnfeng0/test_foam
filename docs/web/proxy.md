# Proxy

翻墙工具发展史：shawdowsocks -> v2ray -> xray

目前使用xray, 各个平台软件介绍如下：

- 服务器端软件：使用脚本快速安装<https://github.com/kirin10000/Xray-script>，需要买一个域名用来解析到服务器的ip地址。伪装域名有助于隐藏服务器的真实ip地址。
- 手机客户端软件：v2rayNG，
- windows客户端软件：v2rayN
- linux PC端：使用xray的二进制文件

除了linux其他平台都比较成熟，这里记录一下linux上面的使用方法。

## 1. 安装xray

使用各个linux发行版的包管理器安装即可。如果没有就用<https://github.com/XTLS/Xray-install>这个脚本安装。

## 2. 配置xray

在任何位置新建一个config.json，里面填写从v2rayN或者v2rayNG导出的配置文件。

原始客户端配置在使用xray-script的配置完成后最后阶段后会打印提示应该如果配置客户端。

## 3. 启动xray

把config.json放到`/etc/xray/config.json`这里是systemd的xray-server的默认配置路径，然后使用xray的二进制文件启动即可。

配置文件的地址可以是任意的，只要在启动的时候指定即可。具体可查看xray-server的systemd配置文件。

安装xray后会有systemd的server配置文件，可以直接使用systemctl启动xray。

```bash
sudo systemctl enable --now xray.service
```

## 4. 使用

然后就可以根据配置文件里面的端口配置来配置系统代理和软件代理了。

- 命令行代理软件：proxychains
- chrome代理插件：SwitchyOmega

自行搜索安装和使用方法即可。