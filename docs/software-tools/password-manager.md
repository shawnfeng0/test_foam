# Password Manager

密码管理器别用闭源的，存在别的服务器上，已有一些服务器密码泄露了。开源领域已经有很多很好的工具了。

目前开源领域最流行的两个密码管理工具是pass和keepassxr。

## pass工具

pass是比较小巧利用了现成的加密工具-gpg加密。然后再自行使用git管理密码版本。
pass工具的概念很好，总是利用现成的工具而不是重复造轮子，很贴合unix工具只做一件事并做好的哲学。

## keepassxr工具

另一个是keepassxr，它是一个命令行工具，界面还算是简洁明了，一看就懂。每个密码还能写一堆备注的信息，也能查看密码历史。算是一个比较齐全的工具了。
而且开源社区还帮忙开发了浏览器插件，可以在网站里自动填充密码。

## 选择

目前准备选择pass工具，顺便学一下gpg，了解一下加密工具。