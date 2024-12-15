# PVE平台下核显物理直通教程

核显直通有 **<u>物理核显直通</u>**、**<u>SR-IOV虚拟核显直通</u>** 两种常规说法，Nasu一开始也是搞混的，如下为两种说法的区别：

![](https://webp.nas-u.top/Nasu_151224151453.png)

本文仅以物理核显直通操作为例，在操作前请确认以满足一下前提条件。

## 一、前提条件：

1. 首先检查BIOS中开启一下功能：
   - Intel VMX虚拟化技术
   - VT-d
   - SR-IOV
   - Above 4GB MMIO BIOS assignment
   - IOMMU
2. 已经正常安装PVE系统，若未安装，具体安装步骤请参考网络教程。

## 二、修改grub文件

利用SSH工具登录PVE后台或者用shell工具进行修改如下文件：

```bash
# 输入命令
nano /etc/default/grub

# 修改其中的 GRUB_CMDLINE_LINUX_DEFAULT="quiet"为如下
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

![](https://webp.nas-u.top/Nasu_151224142207.png)

## 三、编辑pve-blacklist.conf文件

```bash
# 输入如下命令
nano /etc/modprobe.d/pve-blacklist.conf

# 增加下面的
blacklist nvidiafb
blacklist amdgpu
blacklist i915
blacklist snd_hda_intel
options vfio_iommu_type allow_unsafe_interrupts=1
```

![](https://webp.nas-u.top/Nasu_151224142817.png)

## 四、更新initramfs及重启

```bash
# 更新当前系统正在使用内核对应的initramfs镜像文件
update-initramfs -u

# 重启系统
reboot
```

## 五、下载用到的rom文件

Github的gangqizai大神已经针对Intel 核显制作的直通 optionROM，直接下载使用即可。

https://github.com/gangqizai/igd
把 **gen12_gop.rom**、 **gen12_igd.rom** 这两个拷贝到PVE的 /usr/share/kvm 目录下待用。

## 六、新建虚拟机

- 机型必须i440fx
- BIOS必须OVMF，Intel核显已不支持传统BIOS启动
- 显示设置为无
- 虚拟机内存至少4G，小于4G可能有问题
- 添加USB的键盘及鼠标
- 添加核显和声卡通道

![](https://webp.nas-u.top/Nasu_151224141931.png)

## 七、编辑虚拟机文件

```bash
# 编辑conf文件，其中102为上述新建的虚拟机编号
nano /etc/pve/qemu-server/102.conf 

# 在开头增加
args: -set device.hostpci0.addr=02.0 -set device.hostpci0.x-igd-gms=0x2 -set device.hostpci0.x-igd-opregion=on -debugcon file:/root/igd_debug.log -global isa-debugcon.iobase=0x402

# 同时修改
hostpci0: 0000:00:02.0,legacy-igd=1,romfile=gen12_igd.rom
hostpci1: 0000:00:1f.3,romfile=gen12_gop.rom
```

![](https://webp.nas-u.top/Nasu_151224141732.png)

## 八、安装windows并添加核显驱动

- Windows的安装跟通常做法，在此不进行赘述。
- 核显采用官网最新  [核显驱动](https://www.intel.cn/content/www/cn/zh/download/785597/intel-arc-iris-xe-graphics-windows.html)

完成后核显及HDMI声音输出均正常。

![](https://webp.nas-u.top/Nasu_151224144659.png)

## 参考

1. [Intel核显驱动](https://www.intel.cn/content/www/cn/zh/download/785597/intel-arc-iris-xe-graphics-windows.html)
2. [爱折腾的老高](https://www.bilibili.com/video/BV1A94y1p7X6/?spm_id_from=333.999.0.0&;vd_source=4da020b9db51ea30ef4f09bf94e48342)
3. [核显及声卡rom](https://github.com/gangqizai/igd)

