---
title: kvm安装
date: 2021-04-29 10:15:01
tags: [虚拟机]
---

## 一 安装前的检查

> 本机系统为manjaro

### 硬件支持

```
╭─tim@tim-xiaoxin ~ 
╰─$ LC_ALL=C lscpu | grep Virtualization
Virtualization:                  VT-x

```

像这样，有反馈，证明你的硬件支持虚拟化，如果没有反馈，你可以关掉这个文章了。

### 内核支持

```
─tim@tim-xiaoxin ~ 
╰─$  zgrep CONFIG_KVM /proc/config.gz
CONFIG_KVM_GUEST=y
CONFIG_KVM_MMIO=y
CONFIG_KVM_ASYNC_PF=y
CONFIG_KVM_VFIO=y
CONFIG_KVM_GENERIC_DIRTYLOG_READ_PROTECT=y
CONFIG_KVM_COMPAT=y
CONFIG_KVM_XFER_TO_GUEST_WORK=y
CONFIG_KVM=m
CONFIG_KVM_INTEL=m
CONFIG_KVM_AMD=m
CONFIG_KVM_AMD_SEV=y
CONFIG_KVM_MMU_AUDIT=y

```

后面显示**y**或**m**，证明你的内核支持虚拟化，不支持的话，尝试编译内核吧。
确认这些内核模块是否已自动加载

```
╭─tim@tim-xiaoxin ~ 
╰─$ lsmod | grep kvm              
kvm_intel             331776  0
kvm                   933888  1 kvm_intel
irqbypass              16384  2 vfio_pci,kvm

```

若没有反馈，则要加载这些模块

```
modprobe kvm 
modprobe kvm_intel
modprobe kvm_amd   #这里的模块根据自己的cpu选择
```

## 二 QEMU与libvirt虚拟机管理程序安装

我们可以通过`qemu`来使用kvm,而`qemu`的命令行，明显用起来很复杂，我们可以通过`libvirt`来管理`qemu`虚拟机，`libvirt`的客户端多种多样，我们使用用户数量多，功能齐全的`virt-manager`，要使用uefi,要安装`edk2-ovmf`，如果想模拟其他架构，可以安装qemu-arch-extra。

```
sudo pacman -S qemu libvirt virt-manager edk2-ovmf qemu-arch-extra #安装刚才所提到的包
systemctl enable libvirtd #开机自启服务
systemctl start libvirtd #启动服务
```

现在，你的kvm就可以使用了。