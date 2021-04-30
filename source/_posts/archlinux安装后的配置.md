---
title: archlinux安装后的配置
date: 2021-04-28 16:25:41
tags: [archlinux,日常使用]
---

## 添加用户

使用root用户不安全，添加一个普通用户是很有必要的
`useradd -m -G wheel -s /bin/bash  用户名`
设置密码
`passwd 用户名`
编辑/etc/sudoers

```
#%wheel ALL=(ALL) ALL
```

去掉前面的#

```
%wheel ALL=(ALL) ALL
```

退出。

## 图形界面

### 安装`xorg`包组

`sudo pacman -S xorg`

### 安装驱动

#### 显卡驱动

intel：`sudo pacman -S xf86-video-intel`
amd：`sudo pacman -S xf86-video-amdgpu`
ati：`sudo pacman -S xf86-video-ati`
nvidia：`sudo pacman -S nvida #闭源驱动，怕显卡更新崩安装nvidia-dkms`
nvidia（nouveau）：`sudo pacman -S xf86-video-nouveau #nvidia开源驱动`

#### 触控板驱动

`sudo pacman -S xf86-input-libinput`

### 安装桌面环境及显示管理器

#### 安装gnome桌面环境及gdm

```
sudo pacman -S gnome
systemctl enable gdm
systemctl start gdm
```

#### 安装kde及sddm

```
sudo pacman -S plasma sddm
systemctl enable sddm
systemctl start sddm
```

有些人认为`plasma`包组中附带的kde全家桶很臃肿，我们可以安装`plasma-desktop`包获得最小化的kde
作为kde资深用户，我整理了一份kde的安装精简

```
sudo pacman -S akregator dolphin-plugins ffmpegthumbs filelight gwenview kamoso kbackup kcachegrind kcalc kcolorchooser kcron kdeconnect kdegraphics-thumbnailers kdenetwork-filesharing kdesdk-thumbnailers kdf kdialog kfind kgpg kio-gdrive kipi-plugins kirigami-gallery kleopatra kmix kmousetool kmouth kolourpaint kompare krdc kross-interpreters kruler ksystemlog ktimer kwave kwrite okular poxml print-manager svgpart sweeper yakuake zeroconf-ioslave konsole dolphin kate plasma-desktop plasma-nm firefox ark rar ntfs-3g samba
```

这条命令可以安装最小化的kde,同时装了一些日常使用软件。

#### 安装lxde及lxdm

```
sudo pacman -S lxde
systemctl enable lxdm
systemctl start lxdm
```

#### 安装xfce及lightdm

```
sudo pacman -S xfce4 lightdm lightdm-gtk-greeter
systemctl enable lightdm
systemctl start lightdm
```

#### 安装dde及lightdm

```
sudo pacman -S deepin deepin-extra lightdm lightdm-gtk-greeter
```

编辑`/etc/lightdm/lightdm.conf`
找到`greeter-session`
将lightdm主题改为deepin主题

```
greeter-session=lightdm-deepin-greeter
```

## 将系统语言改为中文

### 安装字体

我比较偏爱文泉驿
`sudo pacman -S wyq-microhei wqy-microhei-lite wqy-bitmapfont wqy-zenhei`
gnome和kde可以在系统设置中设置语言

#### 全局中文（可能会tty乱码）

编辑`/etc/locale.conf`

```
LANG=zh_CN.UTF-8
```

编辑`~/.xprofile`

```
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
```

## 添加archlinuxcn源,启用32位源并安装aur助手

编辑`/etc/pacman.conf`，找到`[multilib]`，去掉前面的#（取消注释）

```
[multilib]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist
```

archlinuxcn源我使用压力相对小的北外源
编辑`/etc/pacman.conf`，在最底部添加

```
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
```

安装`archlinuxcn-keyring`包导入 GPG key。
`sudo pacman -S archlinuxcn-keyring`
更新源`sudo pacman -Syyu`
安装aur助手`yay`
`sudo pacman -S yay`

- - - -

 **现在，你有了一个可以正常日常使用的archLinux，尽情享受吧！**