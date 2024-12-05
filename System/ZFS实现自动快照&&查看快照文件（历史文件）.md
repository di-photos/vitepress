# ZFS实现自动快照&&查看快照文件（历史文件）

# 前言

> 在Unraid下，很多人，包括ZFS插件的作者，会选择使用Znapzendzetup去设置ZFS自动快照，但是我发现Znapzendzetup有一些小问题，比如需要perl的包，在Unraid6.10版本上安装繁琐。其次，Znapzendzetup的快照并不能直接在Windows的历史版本中直接查看到，那么文件还原变得麻烦起来。所以，我在寻找了新的方法去实现更好用的自动快照，以及可查看历史文件的功能

![image](https://webp.nas-u.top/image-20241009092233-8vb81zj.png)​

本文参考：

[https://forum.level1techs.com/t/zfs-on-unraid-lets-do-it-bonus-shadowcopy-setup-guide-project/148764](https://forum.level1techs.com/t/zfs-on-unraid-lets-do-it-bonus-shadowcopy-setup-guide-project/148764)

Github源：[https://github.com/zfsonlinux/zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot)

[https://github.com/zfsonlinux/zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot)

# 准备工作

如果本来使用Znapzendzetup创建快照，那么你需要取消Znapzendzetup的自动快照任务

使用如下命令：

`znapzendzetup delete [池名字]/[数据集]`​

示例： 需要取消任务的池为Testpool 数据集为mydata

`znapzendzetup delete Testpool/mydata`​

然后输入命令：

`pkill -HUP znapzend`​

（发生给你的znapzend守护进程中，使它注意到这个变化。）

# 正片

## 下载[zfsonlinux](https://github.com/zfsonlinux)/[**zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot)的脚本\*\*

(也可以直接使用命令：git clone [https://github.com/zfsonlinux/zfs-auto-snapshot.git](https://github.com/zfsonlinux/zfs-auto-snapshot.git))

1. 点击标题进入zfs-auto-snapshot仓库
2. 如图点击下载仓库打包的ZIP文件  
    ​![image](https://webp.nas-u.top/image-20241009092312-hm6osyt.png)​
3. 在你电脑解压后，将文件夹拷贝放入Unraid的目录下（SMB拷贝入，建议放在固定位置）  
    作为示例：  
    我的所有脚本存放路径是：`/mnt/CPool/myscript/`​  
    拷贝放入后，脚本的路径是：`/mnt/CPool/myscript/zfs-auto-snapshot-master/src/zfs-auto-snapshot.sh`​

## 安装&&设置 User Scripts

1. 如图安装

![image](https://webp.nas-u.top/image-20241009092323-lgnsai2.png)​

1. 然后在设置中，我们就能找到User Scripts  
    ​![image](https://webp.nas-u.top/image-20241009092332-rsb22xh.png)​
2. 点击左下角的 `ADD NEW SCRIPT`​
3. 输入你想要命名的脚本名字（随意）  
    ​![image](https://webp.nas-u.top/image-20241009092353-s142iid.png)​  
    我这里示例填入 ： test
4. 如图点击名字左边齿轮 点击 EDIT SCRIPT 编辑脚本  
    ​![image](https://webp.nas-u.top/image-20241009092400-tww6o5k.png)​
5. 在下面填入  
    /mnt/CPool/myscript/zfs-auto-snapshot-master/src/zfs-auto-snapshot.sh // -k 168 --label\=01  
    注意：  
    橙色部分为脚本路径，需要根据自己下载上传到路径改变  
    绿色部分 -k 168 中 168为快照保修数量。

    > 示例：
    >
    > 1. 每小时进行一次快照，需要保留一周， 24 × 7 \= 168，即 -k 168
    > 2. 每天进行2次快照，需要保留两周，2 × 14 \= 28 ，即 -k 28
    >
6. 填写完成后，点击SAVE CHANGE保存脚本  
    ​![image](https://webp.nas-u.top/image-20241009092410-xe7dpto.png)​
7. 保存完成后，我们需要在搭配自己需求，设计脚本执行时间，每执行一次，就进行一次快照  
    ​![image](https://webp.nas-u.top/image-20241009092418-033usix.png)​  
    这里，我是按照步骤6 示例1 中每小时一次快照。所以，我选择 `Scheduled Hourly`​
8. 最后点击左下角的 `APPLY`​ ，即可保存脚本  
    ​![image](https://webp.nas-u.top/image-20241009092426-8pm2w9t.png)​

‍

chmod +x zfs-auto-snapshot.sh

## 设置需要自动快照的数据集

在命令行执行：

zfs set com.sun:auto-snapshot\=true [池名字]/[数据集名字]

示例：

1. 需要将池Testpool的mydata、file 2个数据集设置自动快照  
    zfs set com.sun:auto-snapshot\=true Testpool/mydata  
    zfs set com.sun:auto-snapshot\=true Testpool/file

这样每次脚本执行时，这2个数据集就会进行快照

## 设置SMB共享

1. 在 设置 → SMB → SMB额外 → Samba额外配置 中，修改或者添加如下参数

```jsx
[[共享名称]]
	path=/mnt/[池名称]/[数据集]
	valid users = [用户名]
	write list = [用户名]
	vfs objects = shadow_copy2
	shadow: snapdir = .zfs/snapshot
	shadow: sort = desc
	shadow: format = zfs-auto-snap_%S-%Y-%m-%d-%H%M
```

示例：

需要添加mydate的共享，池为 Testpool 数据集为 mydata，用户名为 spu

```jsx
[Testpool]
	path=/mnt/Testpool/mydata
	valid users = spu
	write list = spu
	vfs objects = shadow_copy2
	shadow: snapdir = .zfs/snapshot
	shadow: sort = desc
	shadow: format = zfs-auto-snap_%S-%Y-%m-%d-%H%M
```

1. **保存**，并**重启SMB**
2. 在Windows添加共享
3. ‍

![image](https://webp.nas-u.top/image-20241009092435-xgtknoc.png)​

在文件夹版本中，已经能查看快照的文件了

需要注意的是，添加SMB共享时，不可直接共享池目录，会导致快照是空，需要添加数据集目录

‍
