# 物理群晖——创建存储空间

**提醒：**

*本系列文章为Nas·U定制产品配套指引，若为自行组装机器仅供参考。内容包括IP获取、系统完善安装、用户创建、文件夹建立、基本设置、常用软件等相关内容。*

### 创建存储空间

1. 进入后台会提示相关操作，按需点击即可（提醒：因为是黑群晖，请不要注册跟官网相关的内容）

![2024-05-31 13 04 51 013  Gemoo Snap.png](https://pic.nas-u.top/3938350344.png)

![2024-05-31 13 06 54 856  Gemoo Snap.png](https://pic.nas-u.top/2672888780.png)

![2024-05-31 13 08 12 035  Gemoo Snap.png](https://pic.nas-u.top/1002399628.png)

2. 提示“创建存储池……”，按立即创建

![2024-05-31 13 09 05 335  Gemoo Snap.png](https://pic.nas-u.top/140905165.png)

3. 如果没看到提示，可以点击左上角的主菜单，选择存储空间管理器

![2024-05-31 13 14 01 887  Gemoo Snap.png](https://pic.nas-u.top/1959028291.png)

![2024-05-31 13 11 45 831  Gemoo Snap.png](https://pic.nas-u.top/1569614786.png)

4. 根据需要，选择磁盘模式。建议选择“SHR”模式；

   *SHR 是群晖独创的阵列，和RAID5类似。该阵列有一个容错硬盘，当硬盘损坏一块的时候，数据也不会丢失，相对来说还是比较安全的。SHR相对RAID5空间利用率会更高。*

![2024-05-31 13 24 48 717  Gemoo Snap.png](https://pic.nas-u.top/3775984703.png)

5. 选择需要的硬盘后，一直点击下一步

   *扩展提示：若**后续增加硬盘**，请参考官方指引*
   [*添加硬盘以扩充存储池容量 | DSM - Synology 知识中心*](https://kb.synology.cn/zh-cn/DSM/help/DSM/StorageManager/storage_pool_expand_add_disk?version=7)

![2024-05-31 13 29 35 647  Gemoo Snap.png](https://pic.nas-u.top/3003423187.png)

6. 点击最大化，按下一步。

![2024-05-31 13 32 54 686  Gemoo Snap.png](https://pic.nas-u.top/821998583.png)

7. 文件系统选择按建议选择“Btrfs”即可。

![2024-05-31 13 34 22 138  Gemoo Snap.png](https://pic.nas-u.top/3648735376.png)

8. 不建议勾选“加密此存储空间”

![2024-05-31 13 36 08 816  Gemoo Snap.png](https://pic.nas-u.top/1567286438.png)

9. 组了存储池组以后，是会进行后台优化的，建议等优化完成后再进行其它操作。

![2024-05-31 13 37 40 218  Gemoo Snap.png](https://pic.nas-u.top/4218395400.png)

