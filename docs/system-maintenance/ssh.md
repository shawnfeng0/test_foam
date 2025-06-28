# Ssh

## 服务器开启SSH服务

在主机上开启sshd服务，一般执行`systemctrl enable --now sshd.server`

开启后即可使用其他主机使用ssh user@ip登录主机。

当主机服务器暴漏在公网上面后，就可能会遭受到任何网络攻击了，我测试能ssh连接后，马上开始把我的本机的短密码修改成了比较长的复杂密码。

还有一些安全注意事项如下：

- 如果允许密码登录，那密码一定要是有一定复杂度的长密码
- 禁止root用户登录
- 密码尝试失败的次数限制，可修改为3次，我的机器默认是6次

## 客户端拷贝ssh密钥

```shell
ssh-copy-id user@ip
```

## windows没有ssh-copy-id命令

使用命令模拟ssh-copy-id:(需要在powershell中执行)

```powershell
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```