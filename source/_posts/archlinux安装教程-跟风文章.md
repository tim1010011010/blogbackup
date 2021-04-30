---
title: archlinux安装教程(跟风文章)
date: 2021-04-28 13:29:01
tags: [archlinux,日常使用]
---

# ArchLinux安装（跟风文章）

## 前言

archlinux的纯文本安装装模式看似复杂，其实，和普通发行版的安装过程几乎相同，所以安装arch没必要紧张，今天记录一下我的安装过程。
（注：archwiki是最好的arch用户手册，安装archlinux的过程请以[archwiki](https://wiki.archlinux.org/index.php/Installation_guide_(简体中文)) 为准

### 准备

一个u盘、耐心、信心、官网上最新的archiso镜像、小学5年级的英语水平。

## 0 一些小准备

### 将iso镜像写入到u盘

Windows下使用软件Rufus，选择dd模式进行写入
Linux下可使用`dd`写入，也可以使用suse studio usb writer这类的软件写入

### 联网

#### 直接用网线联入互联网使用dhcp是一个不错的选择

#### 无线网络

使用iwd连接Wi-Fi

```
device list #列出无线网卡设备
station 网卡名 scan #扫描网络
station 网卡名 get-networks #列出可用网络
station 网卡名 connect Wi-Fi名 #连接Wi-Fi，会要求你输入密码，直接输入即可
```

### ssh连接

很多人可能打字慢，愿意用复制粘贴的方式来安装arch，可以使用ssh连接到电脑进行安装。
先给archiso设置一个密码
`passwd`
启动ssh服务
`systemctl start sshd`
通过`ip a`查看ip地址
利用ssh命令或图形话软件连接到电脑
命令`ssh root@你的ip地址`

## 1 分区

在Windows下装双系统可选择在设备管理器里面提前分好区，然后在archiso里面挂载。
在archiso使用`cfdisk`图形化工具分区。
efi分区作为/dev/sda1，分512mb

### 方案一

单分一个分区作为根目录

### 方案二

小部分的分区作为/，最小15g，剩下的分到/home。

### 关于swap

swap可以根据需求分出分区大小，可以不分，后续使用swapfile也是可以的。

使用`mkfs`进行格式化

```
mkfs.fat -F32 /dev/sdXx #将efi分区格式化为fat32格式
mkfs.ext4 /dev/sdXx #将根目录分区，或者其他分区格式化成ext4格式（建议新手使用）
mkswap /dev/sdXx #将分区作为swap分区。
```

你也可以不只局限于ext4格式，这里在写推荐一些格式
btrfs：内建raid阵列以及快照系统`mkfs.btrfs`
xfs：性能比ext4强劲，更适合大容量硬盘。`mkfs.xfs`
f2fs：擅长小文件的读写，但是大文件读写很慢。`mkfs.f2fs`
可以根据需求选择文件系统，不过，还是要说一句，ext4，yyds！

## 2 挂载

我们要将分好的区挂载到archiso的/mnt下，在/mnt下进行我们的安装。
我们首先要在/mmt下建立/boot文件夹用来挂载boot分区，home分区同理
`mkdir -p /mnt/boot`
`mkdir -p /mnt/home`用于建立挂载home分区的文件夹
然后我们根据我们分好的硬盘分区，逐一进行挂载

```
mount /dev/sdXx /mnt #挂载根目录分区
mount /dev/sdXx /mnt/boot #挂载boot分区
mount /dev/sdXx /mnt/home #挂载home分区
swapon /dev/sdXx #挂载swap分区
```

## 3 安装

由于是网络安装，我们需要更换镜像源，否则不是安装慢，就是丢包。
我们可以手动编辑`/etc/pacman.d/mirrorlist`更换自己喜欢的源
archiso提供了一个自动源排列软件reflector，可以使用这个软件来换源
`reflector —-country China --age 12 —-sort rate —-save /etc/pacman.d/mirrorlist`
接下来就是正式安装啦
`pacstrap /mnt base base-devel linux linux-firmware vim nano networkmanager`
若使用了btrfs，xfs文件系统，lvm，raid，还要安装对应的工具。

## 4 配置系统

生成fstab文件`genfstab -U /mnt >> /mnt/etc/fstab`
从archiso进入系统`arch-chroot /mnt`
开机自动连接网络
`systemctl enable NetworkManager`

设置时区，中国大陆直接上海就可以了`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
生成adjtime文件`hwclock --systohc`

设置语言，我们需要编辑`/etc/locale.gen`，取消掉`zh_CN.UTF-8 UTF-8`的注释（删去前面的#）然后运行`locale-gen`
编辑`/etc/locale.conf`

```
LANG=en_US.UTF-8 #这里暂时使用英文，防止乱码，后期安装桌面环境的时候可以改
```

配置网络方面
`创建/etc/hostname文件，并写入自己心仪的主机名`

```
主机名
```

编辑`/etc/hosts`文件

```
127.0.0.1	localhost
::1		localhost
127.0.1.1	刚才填入的主机名.localdomain 刚才填入的主机名
```

若使用了lvm、raid，需创建Initramfs
`mkinitcpio -P`
设置root用户密码
`passwd`

## 5 安装GRUB引导

### 安装软件包

`pacman -S grub`
若是uefi引导，还需要安装`efibootmanager`，若存在多系统，还需安装`os-prober`

#### 传统bios引导

`grub-install --target=i386-pc /dev/sdX #将grub装入磁盘`

#### UEFI引导

```
mount /dev/sdXx /boot #挂载boot分区
grub-install --target=x86_64-efi --efi-directory=/dev/sdXx --bootloader-id=GRUB #将grub装到你的efi分区
```

### 生成配置文件

`grub-mkconfig -o /boot/grub/grub.cfg`

## 完成

自此，你完成了所有的安装archlinux的步骤，敲入`exit`退出chroot环境，敲入`reboot`重启，然后好好欣赏自己的成果吧。
注：**archwiki是最好的archlinux用户手册**，安装过程请以[archwiki](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 为准，本文仅供参考。