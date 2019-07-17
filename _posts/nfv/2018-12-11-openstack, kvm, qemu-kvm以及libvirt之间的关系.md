---
layout: post
title:  "openstack, kvm, qemu-kvm以及libvirt之间的关系"
date:   2018-12-11 18:55:01 +0800
categories: nfv
---
## KVM(Kernel-based Virtual Machine) 基于内核的虚拟机
KVM是集成到Linux内核的Hypervisor，是X86架构且硬件支持虚拟化技术（Intel VT或AMD-V）的Linux的全虚拟化解决方案。它是Linux的一个很小的模块，利用Linux做大量的事，如任务调度、内存管理与硬件设备交互等。

<figure>
    <img src="{{ site.baseurl }}/images/kvm虚拟化平台架构.jpg" alt="image">
    <figcaption>
        kvm虚拟化平台架构
    </figcaption>
</figure>

 KVM是linux内核的模块，它需要CPU的支持，采用硬件辅助虚拟化技术Intel-VT，AMD-V，内存的相关如Intel的EPT和AMD的RVI技术，Guest OS的CPU指令不用再经过Qemu转译，直接运行，大大提高了速度，KVM通过/dev/kvm暴露接口，用户态程序可以通过ioctl函数来访问这个接口。  
 KVM内核模块本身只能提供CPU和内存的虚拟化，所以它必须结合QEMU才能构成一个完成的虚拟化技术，这就是下面要说的qemu-kvm。
 
## QEMU
QEMU是一套由Fabrice Bellard所编写的模拟处理器的自由软件。它与Bochs，PearPC近似，但其具有某些后两者所不具备的特性，如高速度及跨平台的特性。经由kqemu这个开源的加速器，QEMU能模拟至接近真实电脑的速度。
 
Qemu是一个模拟器，它向Guest OS模拟CPU和其他硬件，Guest OS认为自己和硬件直接打交道，其实是同Qemu模拟出来的硬件打交道，Qemu将这些指令转译给真正的硬件。

<figure>
    <img src="{{ site.baseurl }}/images/qemu.jpg" alt="image">
    <figcaption>
        QEMU架构
    </figcaption>
</figure>

缺点：由于所有的指令都要从Qemu里面过一手，因而性能较差。

## libvirt
libvirt是目前使用最为广泛的对KVM虚拟机进行管理的工具和API。Libvirtd是一个daemon进程，可以被本地的virsh调用，也可以被远程的virsh调用，Libvirtd调用qemu-kvm操作虚拟机。

<figure>
    <img src="{{ site.baseurl }}/images/libvirt.jpg" alt="image">
    <figcaption>
        libvirt架构
    </figcaption>
</figure> 

## KVM和QEMU的关系 
准确来说，KVM是Linux kernel的一个模块。可以用命令modprobe去加载KVM模块。加载了模块后，才能进一步通过其他工具创建虚拟机。但仅有KVM模块是 远远不够的，因为用户无法直接控制内核模块去作事情,你还必须有一个运行在用户空间的工具才行。这个用户空间的工具，kvm开发者选择了已经成型的开源虚拟化软件 QEMU。说起来QEMU也是一个虚拟化软件。它的特点是可虚拟不同的CPU。比如说在x86的CPU上可虚拟一个Power的CPU，并可利用它编译出可运行在Power上的程序。KVM使用了QEMU的一部分，并稍加改造，就成了可控制KVM的用户空间工具了。所以你会看到，官方提供的KVM下载有两大部分(qemu和kvm)三个文件(KVM模块、QEMU工具以及二者的合集)。也就是说，你可以只升级KVM模块，也可以只升级QEMU工具。这就是KVM和QEMU 的关系。

<figure>
    <img src="{{ site.baseurl }}/images/KVM和QEMU关系.png" alt="image">
    <figcaption>
        KVM和QEMU关系
    </figcaption>
</figure> 

## openstack, kvm, qemu-kvm以及libvirt之间的关系

KVM是最底层的hypervisor，它是用来模拟CPU的运行，它缺少了对network和周边I/O的支持，所以我们是没法直接用它的。

QEMU-KVM就是一个完整的模拟器，它是构建基于KVM上面的，它提供了完整的网络和I/O支持。

Openstack不会直接控制qemu-kvm，它会用一个叫libvirt的库去间接控制qemu-kvm。libvirt提供了跨VM平台的功能，它可以控制除了QEMU之外的模拟器，包括vmware, virtualbox， xen等等。

所以为了openstack的跨VM性，所以openstack只会用libvirt而不直接用qemu-kvm。libvirt还提供了一些高级的功能，例如pool/vol管理。

<figure>
    <img src="{{ site.baseurl }}/images/openstack&kvm&qemu-kvm&libvirt之间的关系.png" alt="image">
    <figcaption>
        openstack&kvm&qemu-kvm&libvirt之间的关系
    </figcaption>
</figure> 