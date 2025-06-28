# 打包

python使用pyinstaller工具把需要的 python可执行程序、python脚本、对应的依赖包打包到一个可执行文件中。

## 创建一个干净的python环境

为了减少体积，先使用virtualenv创建一个干净的python环境，在其中使用pip安装必要的库

<https://virtualenv.pypa.io/en/latest/user_guide.html>

### 没有网络的环境使用virtualenv

下载一个打包后的virtualenv
<https://virtualenv.pypa.io/en/latest/installation.html#via-zipapp>

## 安装必要的依赖库

pip install -r ./requirements.txt

## 使用pyinstaller打包可执行程序

pyinstaller在各个平台（windows、linux）上的打包是独立的，在linux平台上只能打包linux的二进制，无法跨平台打包。

命令参考：
pyinstaller -F xxx.py
