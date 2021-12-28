---
title: deepin安装注意事项和安装后配置
date: 2021-12-28 20:42:48
categories:
 - 安装和配置
tags:
 - deepin
---

* 安装时分区注意;
* 使用tmpfs;
* 配置vnc和xrdp远程;
* 快捷方式;
* 应用商店报错修复;
* 常用软件安装(go,mariaDB,npm,firefox,安卓模拟器);

<!-- more -->

# 安装时分区注意

不要用默认分区,不要用默认分区,不要用默认分区.

千万注意,默认分区只给根目录分配`15G`空间,一下子就满了,满了很难搞,非常折磨

我的分区方式如下

| 挂在目录 | 文件系统 | 大小 |
| --- | --- | --- |
| `/boot/efi` | `efi` | `200M` |
| `/Boot`(windows相关) | `ext4` | `2G` |
| `/` | `ext4` | `其余全部空间` |
| `<无>` | `linux-swap` | `8G` |


# 移动/tmp目录到tmpfs

编辑`/etc/fstab`文件,后面加上以下内容

```shell
tmpfs /tmp tmpfs defaults 0 0
tmpfs /var/tmp tmpfs defaults 0 0
```

# 配置远程桌面

{% post_link deepin开启远程桌面服务-使用vnc和rdp连接deepin %}

# 快捷方式

## 制作快捷方式

当我们有一个可执行文件,希望能够在开始菜单直接打开他.

首先可以编写一个文件`<文件名>.desktop`,来制作快捷方式.

```shell
[Desktop Entry]
Exec=<可执行文件地址>
Name=<快捷方式显示的名称>
Icon=<网上找的图标.svg>
Type=Application
```

以上都是绝对路径,比如

```shell
[Desktop Entry]
Name=Uengine
Exec=/home/zzidun/app/uengine/uengine.sh
Icon=/home/zzidun/app/uengine/anbox.svg
Type=Application
```

## 快捷方式移动到开始菜单

复制到`/usr/share/applications`目录即可.

```shell
sudo cp <xxx.desktop> /usr/share/application/
```

# 应用商店安装到75%报错

一般是由于卸载软件没有完成导致的,用以下命令可以解决:

```shell
sudo apt-get --fix-broken install -y
```

# 常用软件安装

## git设置

```shell
git config --global user.email "<邮箱>"
git config --global user.name "<用户名>"
git config --global http.postBuffer 10000M
```

## go

```shell
wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz

sudo tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
```

然后修改文件`~/.bashrc`

加上这一句
```shell
export PATH=$PATH:/usr/local/go/bin
```

然后
```shell
source ~/.bashrc
```

检查是否安装完成

```shell
go version
```

## mariaDB

{% post_link 自行安装nodejs和npm-解决apt源版本不兼容问题 %}

## npm

{% post_link deepin安装和配置mariaDB %}

## firefox

缩小顶边栏

```
顶部右键 -> Customisze Firefox -> 屏幕靠下TitleBar取消打勾
```

使用内存作为缓存

火狐地址栏输入`about:config`,进入配置页面.

搜索栏输入`browser.cache.disk.enable`设置为`false`.


搜索栏输入`browser.cache.memory.enable`设置为`true`.


搜索栏输入`browser.cache.memory.capacity`设置为`-1`.


## 安卓模拟器

安装uengine
```shell
sudo apt install uengine
```

安装apk
```shell
uengine install --apk=<apk的绝对路径>
```

启动模拟器
```shell
uengine launch --package=org.anbox.appmgr --component=org.anbox.appmgr.AppViewActivity
```

## platformIO

直接`vscode`安装对应插件.

重新启动`vscode`.

默认情况下,platformIO会把新建的项目放到C盘用户文件里面某个很深的路径.

修改新建项目时保存路径(在pio的终端输入)

```shell
pio settings set projects_dir <路径>
```