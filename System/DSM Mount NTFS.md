# 群晖 DSM 挂载 NTFS 硬盘

群晖默认支持的文件系统有 btrfs 和 ext4。相对于 WINDOWS 下常用的 NTFS 格式，群晖对其支持存在一定的局限性。群晖内部硬盘不支持 NTFS 格式，仅支持外部设备如插在 USB 口上的移动硬盘的 NTFS 格式。例如，在数据管理、使用、恢复等方面，NTFS 格式对于一些用户来说更为熟悉和方便，但在群晖系统中，却不能直接将 NTFS 格式硬盘作为内部硬盘使用。这给一些用户带来了不便，尤其是那些有大量数据存储需求且习惯使用 NTFS 格式的用户。其实，可以通过计划任务来实现群晖对 NTFS 硬盘的读取支持，但也需要用户具备一定的技术知识和操作经验。该方法在群晖918+的7.2.1版本实践成功，供各用户参考。

## DSM 计划任务挂载

##### 一、插入 NTFS 硬盘并通过 SSH 定位盘符。

- 1. 首先要在群晖的 **控制面板**——**终端机和 SNMP**——**启动 SSH** 功能 打勾。

     ![](https://webp.nas-u.top/Nasu_241027220724.png)

- 2. 利用ssh工具（如FinalShell）登录群晖，输入sudo -i 命令切换到root用户。

     ![](https://webp.nas-u.top/Nasu_241027220217.png)

  

- 3. 查找挂载分区，执行fdisk -l命令。

     ![](https://webp.nas-u.top/Nasu_241027220520.png)

- 4. 在Device设置这一栏找到微软对应文件相应盘符，并拷贝设备名称，比如截图的/dev/sdd5。

     ![](https://webp.nas-u.top/Nasu_241027215806.png)

##### 二、新建挂载文件夹便于映射。

- 1. 在控制面板新建共享文件夹，如命名为Ntfs_Share。

     ![](https://webp.nas-u.top/Nasu_241027221414.png)

- 2. 然后在FileStation创建子文件夹对应 NTFS 硬盘的盘符，如：Disk01。

     ![](https://webp.nas-u.top/Nasu_241027222018.png)

- 3. 选中新建的文件夹右键，选择属性，拷贝绝对路径以便后续使用。

     ![](https://webp.nas-u.top/Nasu_241027222316.png)

##### 三、在计划任务脚本中设置开机挂载，创建任务并运行，测试挂载是否成功。

- 1. 进入控制面板的任务计划，新增用户自定义脚本。

     ![](https://webp.nas-u.top/Nasu_241027222620.png)

- 2. 在常规设置中，任务名称可自定义，用户帐号设置为root，事件选择开机并勾选已启动。

     ![](https://webp.nas-u.top/Nasu_241027222720.png)

- 3. 任务设置中，定义脚本为mount -t ntfs  ***SSH 查询到的 NTFS 盘符*** ***File Station创建的挂载文件夹***，如下面所示：

     ```
     mount -t ntfs /dev/sdd5 /volume3/Ntfs_Share/Disk01
     ```

     ![](https://webp.nas-u.top/Nasu_241027223221.png)

- 4. 选中该任务**右键**，在弹出来的菜单中选择 **运行**。

     ![](https://webp.nas-u.top/Nasu_241027223452.png)

- 5. 最后回到File station查看是否成功挂载，可通过拷贝文件来测试能否直接成功拷贝。

     ![](https://webp.nas-u.top/Nasu_241027224931.png)

     

