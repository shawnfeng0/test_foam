# Sudo without Password

配置某个用户或用户组使用sudo时免输密码，需要修改/etc/sudoers配置。

根据网上的说法在和/etc/sudoers文件的注释说明在/etc/sudoers里面添加一下配置即可：

```shell
## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL:ALL) ALL

## Same thing without a password
# %wheel ALL=(ALL:ALL) NOPASSWD: ALL
```

前面如果是用户组就用%开头，用户就直接写用户名。

在manjaro上面配置的时候发现没生效，后来发现配置是被覆盖了。

后来发现是/etc/sudoers最后一行`@includedir /etc/sudoers.d`引入了新的配置，查看sudoers.d目录，有一个10-installer文件，估计是系统安装的时候生成的文件。10-installer内容如下：

```shell
%wheel ALL=(ALL) ALL
```

我的用户名也在wheel用户组里，所以配置被这个覆盖了。

修改的方法是在/etc/sudoers的最后添加配置，这样就谁也不能覆盖了。
或者是在sudoers.d目录添加新的配置文件，但是要保证在10-installer之后加载，我猜测文件的前缀数字是优先级的概念，但是我还没有验证这种方法。

修改完后的/etc/sudoers最后两行如下：

```shell
## Read drop-in files from /etc/sudoers.d
@includedir /etc/sudoers.d
shawnfeng ALL=(ALL:ALL) NOPASSWD: ALL
```