---
description: 重新安装Linux时的配置和快捷键
---

# Linux Tips



**设置root账户的密码**

```text
sudo passwd root
```

**vi编辑模式下Backspace无法退格删除Ref**

```text
set nocp
```

**vi编辑模式下上下左右出现字母**

```text
sudo vi  /etc/vim/vimrc.tiny
```

修改 set compatible 为 set nocompatible 设置是否兼容 添加 set backspace=2 设置 backspace可以删除任意字符

**root切换**

* su \#进入root
* su fred  \#su 用户名

  **无法连接虚拟设备 sata0:1，因为主机上没有相应的设备**

  破解版的 VMware Fusion, 关机-&gt;设置-&gt;硬盘-&gt;高级选项-&gt;总线类型:SATA

  **配置scp传送文件**

**Ubuntu内建命令Ref**

```text
apt-get XXX
```

删除

```text
dd #删除整行
d1G #删除当前行到顶
dG #删除当前行到底
```

**Bash**

* 文件扩展名为.bash
* **!/bin/bash, 在宣告这个scripts内的语法是bash**
* **注解**
* echo = printf, -e = 开启转义

