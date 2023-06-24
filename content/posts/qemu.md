---
title: "qemu"
date: 2023-06-18
draft: false
tags : [                    # 文章所属标签
    "Cloud",
    "SDN",
]
categories : [              # 文章所属标签
    "技术",
]
---


# qemu概述

> https://blog.csdn.net/weixin_38387929/article/details/120121636

## qemu的几个特点:

- qemu可以被当做模拟器,也可以被当做虚拟机
- 当qemu被当做模拟器时,我们可以在一台设备上通过模拟设备,运行针对不同于本机上CPU的程序或者操作系统
- 当qemu被当做虚拟机使用时,qemu必须基于Xen Hypervisor或者kvm内核模块才能支持虚拟化.在这种条件下qemu虚拟机可以直接在本机CPU上运行客户机代码获得接近本机的性能.

# Qemu和KVM的关系

- 当qemu在模拟器模式下,运行操作系统时,我们可以认为这是一种软件实现的虚拟化技术,它的效率比真机差很多,用户可以明显的感觉出来
- 当qemu在虚拟机模式下,qemu必须在linux上运行,并且需要借助kvm或者xen,利用intel或者AMD提供的硬件辅助虚拟化技术,才能使虚拟机达到接近真机的性能.
- qemu与kvm内核模块协同工作,在虚拟机进程中,各司其职,又相互配合,最终实现高效的虚拟机应用

# qemu与kvm的关系如下

![Description](https://www.mineor.xyz/images/20230618/qemuandkvm.png)

- kvm在物理机启动时创建/dev/kvm设备文件,当创建虚拟机时,kvm为该虚拟机进程创建一个vm的文件描述符,当创建vCPU时,kvm为每个vCPU创建一个文件描述符.
- 同时,kvm向用户空间提供了一系列针对特殊设备文件的ioctl系统调用.qemu主要通过ioctl系统调用与kvm进行交互的. 

# qemu与kvm等虚拟化的关系

qemu有几种虚拟化模式

首先,它可以使用基于内核的虚拟机(kvm)执行x86处理器硬件虚拟化,以几乎比拟硬件本机的速度执行运算任务.
其次,他可以通过机器点啊的实时转换来模拟其他处理器以用于虚拟机运行在不同平台的操作系统.
最后,他可以实时转换为其他架构运行简单的程序,类似于linux的wine.

因为QEMU没有图形用户界面（GUI），而其提供的核心能力又是关键而重要的，因此通常用作更复杂的虚拟化管理器的一部分。比如，我们经常使用的开源VirtualBox、Xen虚拟化产品，其核心底层的虚拟化部分就有集成和使用QEMU，此外，主流的KVM虚拟化也是集成和使用QEMU的主力虚拟化管理器系统。

从KVM的角度来说，KVM（Kernel Virtual Machine）是Linux的一个内核驱动模块，它能够让Linux主机成为一个Hypervisor（虚拟机监控器）。在支持VMX（Virtual Machine Extension）功能的x86处理器中，Linux在原有的用户模式和内核模式中新增加了客户模式，并且客户模式也拥有自己的内核模式和用户模式，**虚拟机就是运行在客户模式中**。KVM模块的职责就是打开并初始化VMX功能，提供相应的接口以支持虚拟机的运行。KVM通过调用Linux本身内核功能，实现对CPU的底层虚拟化和内存的虚拟化，使Linux内核成为虚拟化层。KVM在2007年2月被导入Linux 2.6.20内核中。从存在形式来看，它包括两个内核模块：kvm.ko和kvm_intel.ko（或kvm_amd.ko），本质上，KVM是管理虚拟硬件设备的驱动，该驱动使用字符设备/dev/kvm（由KVM本身创建）作为管理接口，主要负责vCPU的创建、虚拟内存的分配、vCPU寄存器的读写以及vCPU的运行。

从QEMU的角度来说，QEMU（Quick Emulator)本身并不包含或依赖KVM模块，而是一套由Fabrice Bellard编写的模拟计算机的自由软件。QEMU虚拟机是一个纯软件的实现，可以在没有KVM模块的情况下独立运行，但是性能比较低。QEMU有整套的虚拟机实现，包括处理器虚拟化、内存虚拟化以及I/O设备的虚拟化。在不需要KVM加速的情况下，QEMU通过一个特殊的“重编译器”对特定的处理器的二进制代码进行翻译，从而具有了跨平台的通用性。QEMU有两种工作模式：系统模式，可以模拟出整个电脑系统，另一种是用户模式，可以运行不同与当前硬件平台的其他平台上的程序（比如在x86平台上运行跑在ARM平台上的程序）。目前最新版本是4.x。从QEMU角度来看，虚拟机运行期间，QEMU通过KVM模块提供的系统调用接口进行内核设置，由KVM模块负责将虚拟机置于处理器的VMX模式运行。QEMU使用了KVM模块的虚拟化功能，为自己的虚拟机提供硬件虚拟化加速以提高虚拟机的性能。

而现在流行的KVM虚拟化平台，就是在修改了QEMU代码，把他模拟CPU、内存的代码换成KVM，而网卡、显示器等留着，因此QEMU+KVM就成了一个完整的虚拟化平台。由于KVM运行在内核空间，只是内核模块，QEMU运行在用户空间，实际模拟创建，管理各种虚拟硬件（磁盘，网卡，显卡等）。从KVM的角度来说，用户没法直接跟内核模块交互，需要借助用户空间的管理工具，因此需要借助QEMU这个运行在用户空间的工具。KVM和QEMU相辅相成，QEMU通过KVM达到了硬件虚拟化的速度，而KVM则通过QEMU来模拟设备并实现和内核空间的KVM的交互，虽然这个交互并不仅仅只有QEMU能够办到。此外，由于QEMU模拟IO设备效率不高的原因，现在常常采用半虚拟化的virtio方式来虚拟IO设备。

综上，理解了QEMU和KVM的关系，也就理解了VirtualBox、Xen等虚拟化产品集成和使用QEMU的关系。

# 硬件设备

在qemu中,存在两种使用硬件设备的方式:
- **直通模式**使用主机实际物理设备
- **qemu的设备驱动仿真**实现的模拟虚拟设备.

如果采用直通方式使用实际的物理设备,那么就会抢占主机的设备使用权,并且其他虚拟机也将无法使用该物理设备.
在直通模式中,虚拟机可以直接访问usb总线或pci总线,并可以直接与设备通信.
一般情况下,采用直通模式的物理设备都是很难进行qemu仿真的设备,比如网络摄像头,串行和并行端口等.
其他设备因为大部分虚拟机都会使用,并且很难与主机共享,例如网络设备,因此大部分都会使用qemu模拟仿真的虚拟设备.比如在虚拟机的网络设备中,可以通过模拟网卡来解决,从而在网络堆栈上添加额外的层.
此外,qemu可以选择连接到linux内核中的"virto"半虚拟化驱动程序,这意味着linux内核处理虚拟机和硬件设备之间的输入/输出,而不采用qemu的模拟设备进行中转和传输(仅用作中介).

# 磁盘映像

qemu可以处理几种不同的磁盘映像格式.
首选为raw或qcow2
Raw是一种非常简单的格式，它将文件系统中的字节逐字节存储在文件中。大多数其他仿真器都支持此格式。
Qcow2是QEMU自己的图像格式，对小图像很有用，并且支持磁盘映像压缩以及捕获磁盘映像状态的快照。
还支持另外两种格式：在VirtualBox中使用的vdi和在VMWare中使用的vmdk。