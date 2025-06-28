# Home Server

最近发现我家里的宽带有固定的IP地址，于是我开始着手把家里的电脑搞成一台能外部连接的服务器。

## 路由器配置-开启DMZ主机配置

DMZ介绍：<https://en.wikipedia.org/wiki/DMZ_(computing)#:~:text=A%20home%20router%20DMZ%20host,host%20from%20the%20internal%20network.>

wiki上说这个词是误用，我们暂且不管，只需要知道意义是：在路由器里把某个连接的设备指定为DMZ主机之后，这台主机就可以完全暴露在公网上面了。

具体不同的路由器设置不一样，自行查询是否支持。

## SSH配置

参考[Ssh](/system-maintenance/ssh.md)

## 防火墙配置

参考[Firewall](/system-maintenance/linux/firewall.md)

## 服务器配置资源监控

我用ssh连接家里服务器的时候，登录欢迎语里面提示我`Activate the web console with: systemctl enable --now cockpit.socket`，一看有web console就有兴趣了，搜索了一下，启用之后打开<https://IP:9090>网页。里面能看到服务器的各种状态信息，还有错误日志，我就是从错误日志里面发现有人尝试通过ssh登陆我的服务器。

## 安装code-server远程开发

code-server安装参考：<https://coder.com/docs/code-server/latest/install#arch-linux>

使用`systemctl --user enable --now code-server.service`命令开启code-server服务，systemctl --user指对应用户第一次登录时才开启服务。

`~/.config/code-server/config.yaml`文件是code-server的配置文件，里面可以配置监听的端口和IP地址，默认为`127.0.0.1:8080`本地监听，我修改成了`0.0.0.0:9100`可外部访问。

code-server使用现有vscode的插件的方法：<https://coder.com/docs/code-server/latest/FAQ#how-can-i-reuse-my-vs-code-configuration>

## 添加https服务器 反向代理code-server服务和cockpit服务

使用反向代理可以把本地的多个服务比如code-server和cockpit用同一个域名访问，只需要在域名后面区分sub-directory即可，并且只需要主服务器有https即可，其他的服务都能享受到https，原本的服务只需要开放本地http访问，增加了安全性，反向代理是http服务器的常用功能。

常用的http服务器是nginx和caddy，caddy配置简单更加现代化，并可以自动获取tls证书，虽性能不如nginx，但如果只是简单的服务用caddy是比较好的选择。服务器对比参考：<https://blog.logrocket.com/comparing-best-web-servers-caddy-apache-nginx/>

caddy的配置文件是`/etc/caddy/Caddyfile`，在域名服务器上先把dns配到我的服务器IP上，配置了caddy后发现无法连接，检查了ufw防火墙配置也都开了，搜索了下原来是家用宽带一般会被运营商屏蔽80和443端口。

caddy必须使用https链接，并且证书要签名才能用，caddy服务起来后发现没有签名会自动跑签名流程，在cockpit里面可以看到caddy服务的日志输出，显示一直获取签名失败。

````
根据caddy的文档描述：https://caddyserver.com/docs/automatic-https#overview 必须开启80和443端口且能外部访问，不然[acme-challenges](https://caddyserver.com/docs/automatic-https#acme-challenges)会失败，caddy会使用三种方式执行acme-challenges，http challenge是使用80端口访问目标服务器以验证目标服务器正确。tls-alpn challenge使用443端口验证目标服务器的正确性。由于这两个端口都被禁用了，所以这两种方式都行不通，还有一种方式是DNS challenge，需要在用域名服务商那里提供一个DNS txt记录，如果本地的和服务商那里填的一样就证明这域名是你的。

caddy有一些对应域名供应商的插件可以自动做dns-challenge，我用的是porkbun域名供应商，caddy有社区版的dns辅助插件：<https://github.com/caddy-dns/porkbun>，caddy有下载界面<https://caddyserver.com/download>可以下载带有任意数量插件的二进制版本，但是这种方式不是很方便。尝试调用`caddy --help`发现有一个add-package的选项，这是一个实验性功能，可以在本地用命令安装插件：`sudo caddy add-package github.com/caddy-dns/porkbun`，执行后安装成功，按照插件主页的介绍，填写在porkbun上申请到的 pubkey 和 seckey。

执行`sudo systemctl reload caddy`重新加载配置文件，在cockpit上查看caddy的日志发现获取签名成功。使用<https://home.shawnwall.org:8443>在浏览器成功打开caddy的界面。

caddy的完整配置如下：

```caddy
{
        http_port 8080
        https_port 8443
        # Restrict the admin interface to a local unix file socket whose directory
        # is restricted to caddy:caddy. By default the TCP socket allows arbitrary
        # modification for any process and user that has access to the local
        # interface. If admin over TCP is turned on one should make sure
        # implications are well understood.
        admin "unix//run/caddy/admin.socket"
        acme_dns porkbun {
                        api_key pk1_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
                        api_secret_key sk1_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
        }
}

home.shawnwall.org {
        # Set this path to your site's directory.
        root * /usr/share/caddy

        # Enable the static file server.
        file_server

        # Another common task is to set up a reverse proxy:
        # reverse_proxy localhost:8080
        reverse_proxy /cockpit/* localhost:9090 {
                transport http {
                        tls_insecure_skip_verify
                }
        }

        # Or serve a PHP site through php-fpm:
        # php_fastcgi localhost:9000

        # Refer to the directive documentation for more options.
        # https://caddyserver.com/docs/caddyfile/directives
}

home.shawnwall.org/code/* {
        uri strip_prefix /code
        reverse_proxy localhost:9100
}

# Import additional caddy config files in /etc/caddy/conf.d/
import /etc/caddy/conf.d/*
````

code-server使用caddy反向代理的方法文档：<https://coder.com/docs/code-server/latest/guide#using-lets-encrypt-with-caddy>

cockpit使用caddy反向代理的方法文档：<https://caddy.community/t/example-cockpit/8283>，不止需要配置caddy，还需要配置cockpit。cockpit是监控工具，所以默认就启用了https访问，只不过是没经过认证的签名。

### 文件服务器配置

#### 配置某些文件服务root无法访问，可能是权限问题

<https://caddy.community/t/how-to-solve-status-403-in-caddy-version-2/9570/2>

另一种解释：<https://caddy.community/t/403-forbidden-systemd-no-selinux-arch-linux-no-var-www/13714>
说在caddy的systemd配置里面默认配置了ProtectHome=true，所以home目录的访问被systemd拦截了

#### 文件服务器使用子目录

<https://caddy.community/t/v2-file-server-subdirectory-with-listings/7711/2>
<https://github.com/caddyserver/caddy/pull/3281>

#### 仅开启http

这些方式试了一遍，都不行。

<https://stackoverflow.com/questions/62896495/caddy-how-to-disable-https-only-for-one-domain>
<https://caddy.community/t/is-there-any-way-to-disable-tls-from-the-caddyfile/8372>