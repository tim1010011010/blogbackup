---
title: Linux常用包管理器使用.md
date: 2021-04-28 09:57:03
tags: [日常使用]
---

# Linux常用包管理器使用汇总

## Debian 系 apt （dpkg）

```
apt install xxx #安装软件
dpkg -i xxx.deb #从软件包安装软件
apt update #更新软件源
apt upgrade #更新软件、系统
apt search xxx #搜索软件包
apt remove xxx #删除软件包保留配置文件
apt purge xxx #删除软件包且清除配置文件
apt clean #清除缓存
```

![](https://img2.baidu.com/it/u=1439627636,4061723749&fm=26&fmt=auto&gp=0.jpg)
![](https://img0.baidu.com/it/u=1238697687,1778956956&fm=26&fmt=auto&gp=0.jpg)

## Arch Linux系 paman

```
pacman -S xxx #安装软件
pacman -U xxx.tar.xz #从软件包安装软件
pacman -Sy #更新软件源
pacman -Syy #强制更新软件源
pacman -Su #更新软件、系统
pacman -Ss #搜索软件包
pacman -Rscn #R代表删除软件包，s为删除只与这个包有关的依赖，c为删除所有与这个包有关的依赖，n为删除备份文件
paccache -r #清理缓存
```

### aur助手使用

aur助手用法与pacman用法相同
![](https://img2.baidu.com/it/u=2578618453,1013574475&fm=26&fmt=auto&gp=0.jpg)

![](https://img2.baidu.com/it/u=790418540,3463877127&fm=26&fmt=auto&gp=0.jpg)

## redhat系 yum（rpm）

```
yum install xxx #安装软件
yum remove xxx #删除包及其依赖
yum update #更新软件包
yum upgrade #大规模更新
yum search xxx #查找软件包
yum info xxx #显示安装包rpm的详细信息
yum list updates #列出可更新的rpm包
yum clean all #清理缓存
yum -y #安装过程中自动选择y
```

![](https://img0.baidu.com/it/u=448269321,917584893&fm=26&fmt=auto&gp=0.jpg)
![](https://img2.baidu.com/it/u=1258533790,1699764673&fm=26&fmt=auto&gp=0.jpg)

## fedora包管理器 dnf（rpm）

```
dnf install xxx #安装软件包
dnf update #更新软件、系统
dnf upgrade #与update一样都是更新
dnf search xxx #搜索软件包
dnf remove xxx #删除软件包
dnf erase xxx #与remove一样都是删除软件包
dnf clean #清理缓存
```

![](https://img0.baidu.com/it/u=753584436,473928183&fm=26&fmt=auto&gp=0.jpg)

## openSUSE包管理器zypper（rpm）

```
zypper install xxx
zypper in xxx #安装软件包
zypper update
zypper up #更新
zypper dist-upgrade
zypper dup #更新系统（大规模更新）
zypper search xxx
zypper se xxx #搜索软件包
zypper remove xxx
zypper rm xxx #删除软件包
zypper clean #清理缓存
```

### obs个人源软件包安装

`opi xxx`

![](https://img0.baidu.com/it/u=3164329455,2590160476&fm=26&fmt=auto&gp=0.jpg)

- - - -

参考[distrowatch](https://distrowatch.com/dwres.php?resource=package-management)