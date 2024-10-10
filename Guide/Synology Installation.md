# NAS使用指引 | 物理群晖

本文为博主定制产品配套指引，若为自行组装机器仅供参考。内容包括IP获取、系统完善安装、用户创建、文件夹建立、基本设置、常用软件等相关内容。

## 一、查找群晖IP

1. 安装好硬盘并插上U盘，连接电源线、网线，开机。
2. 查找群晖有四种方法：

- #### 方法一（推荐）：
  
  ①下载群晖助手并安装
  https://global.synologydownload.com/download/Utility/Assistant/7.0.4-50051/Windows/synology-assistant-7.0.4-50051.exe?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057
  
  ②打开群晖助手，一般情况下会自动显示局域网内群晖设备，如果没显示请点搜索，等显示群晖IP地址等相关信息后双击。

![2024-05-31 11 01 26 641  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2809347334.png)

- #### 方法二：

  直接在浏览器输入[http://find.synology.com](http://find.synology.com/)，会自动帮你查找NAS设备，点连接即可。

- #### 方法三：
  
  连接显示器，2分钟左右屏幕会显示设备IP，浏览器输入 "http://设备IP:5000"，如：[http://10.10.10.20:5000](http://10.10.10.20:5000/)

![2024-05-31 11 03 57 27  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2598487719.png)

- #### 方法四：

  进入路由器后台，找到新加入的设备IP，浏览器输入 "http://设备IP:5000"，如：[http://10.10.10.20:5000](http://10.10.10.20:5000/)

## 二、安装群晖系统

1. 在开始安装前先从官方网站下载好安装镜像(该指引以SA6400型号7.2.1版本为例，请在下载前确认相应型号及版本)。
   https://global.synologydownload.com/download/DSM/release/7.2.1/69057-1/DSM_SA6400_69057.pat?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057
2. 根据IP进入安装界面，点击安装。

![2024-05-31 11 19 57 989  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/259606661.png)

3. 选择“从计算机手动上传 .pat 文件”，浏览并选择刚才下载的镜像。

![2024-05-31 11 23 43 401  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/798834223.png)

4. 勾选“我了解……”，按 继续

![2024-05-31 11 25 54 463  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2236607828.png)

5. 输入产品型号（具体以你产品的型号修改，如SA6400、920+）,按继续

![2024-05-31 11 28 45 135  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2580480030.png)

6. 按提示安装，等待10来分钟左右

![2024-05-31 11 31 34 021  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/987029697.png)

![2024-05-31 11 32 09 24  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2260209449.png)

7. 安装好会自动重启，并进入设置界面

![2024-05-31 12 24 30 088  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2446858020.png)

8. 填入名称、用户名、密码等相关信息，按下一步

![2024-05-31 12 26 22 114  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/779820719.png)

9. 选择“……手动安装”选项，按下一步

![2024-05-31 12 52 31 972  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/790509920.png)

10. 黑群晖就不要注册官网账号了，选择“跳过”

![2024-05-31 12 54 57 702  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2608843553.png)

11. **不勾选**“我同意……”，按提交

![2024-05-31 12 57 11 581  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/689344719.png)

12. 稍后会自动打开登录页面，会提示不是“……不是专用连接”（HTTPS连接的安全机制是需要证书，因为我们是局域网本地连接，可以忽略），点 高级，继续点“继续访问 XX.XX.XX.XX(不安全)”即可进入登录页面，输入刚才的用户名及密码登录。

![2024-05-31 13 00 10 507  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/4011025219.png)

![2024-05-31 13 02 47 275  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/464725226.png)

## 三、创建存储空间

1. 进入后台会提示相关操作，按需点击即可（提醒：因为是黑群晖，请不要注册跟官网相关的内容）

![2024-05-31 13 04 51 013  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3938350344.png)

![2024-05-31 13 06 54 856  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2672888780.png)

![2024-05-31 13 08 12 035  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1002399628.png)

2. 提示“创建存储池……”，按立即创建

![2024-05-31 13 09 05 335  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/140905165.png)

3. 如果没看到提示，可以点击左上角的主菜单，选择存储空间管理器

![2024-05-31 13 14 01 887  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1959028291.png)

![2024-05-31 13 11 45 831  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1569614786.png)

4. 根据需要，选择磁盘模式。建议选择“SHR”模式；
   
   *SHR 是群晖独创的阵列，和RAID5类似。该阵列有一个容错硬盘，当硬盘损坏一块的时候，数据也不会丢失，相对来说还是比较安全的。SHR相对RAID5空间利用率会更高。*

![2024-05-31 13 24 48 717  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3775984703.png)

5. 选择需要的硬盘后，一直点击下一步

   *扩展提示：若**后续增加硬盘**，请参考官方指引*
   [*添加硬盘以扩充存储池容量 | DSM - Synology 知识中心*](https://kb.synology.cn/zh-cn/DSM/help/DSM/StorageManager/storage_pool_expand_add_disk?version=7)

![2024-05-31 13 29 35 647  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3003423187.png)

6. 点击最大化，按下一步。

![2024-05-31 13 32 54 686  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/821998583.png)

7. 文件系统选择按建议选择“Btrfs”即可。

![2024-05-31 13 34 22 138  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3648735376.png)

8. 不建议勾选“加密此存储空间”

![2024-05-31 13 36 08 816  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1567286438.png)

9. 组了存储池组以后，是会进行后台优化的，建议等优化完成后再进行其它操作。

![2024-05-31 13 37 40 218  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/4218395400.png)

## 四、取消https安全提示

因为是在局域网内部访问，所以可以用http协议访问。点击桌面控制面板，选择“登录门户”，取消勾选“自动将……”点保存。后续就可以用http://设备IP：5000 就不会有安全提示了（提醒：用5000端口，前面的http是没有“s”的）

![2024-05-31 13 59 57 393  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1547127057.png)

![2024-05-31 14 01 35 399  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/554679194.png)

![2024-05-31 14 03 45 682  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/783911300.png)

## 五、创建共享文件夹

1. 点击控制面板的共享文件夹，新增加一个共享文件夹，用来存放各种常用的数据，如xx_Works、xx_Files等（支持中文名，但建议用英文名）

![2024-05-31 14 18 16 307  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2174458883.png)

2. 选择存储空间，其他设置按默认设置，一路点击下一步。

![2024-05-31 14 21 45 031  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/935513822.png)

3. 需要根据每个用户配置共享文件夹的权限，完成共享文件夹的创建。

![2024-05-31 14 45 41 784  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2844202061.png)

4. 创建了共享文件夹后，打开我的电脑的地址栏，输入`\\ + NAS IP地址`。如`\\10.10.10.39`，输入用户名密码后，可访问NAS存储空间。

![2024-05-31 14 48 30 48  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/830443268.png)

![2024-05-31 14 51 37 46  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2422361974.png)

5. 常用文件夹可以右键点击 映射网络驱动器，创建完成就可以当成本地磁盘用。

![2024-05-31 14 56 54 853  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3846710211.png)

![2024-05-31 14 58 17 699  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/623638222.png)

## 六、新增用户

1. 依次控制面板、用户和群组、用户账号、新增、创建用户。

![2024-05-31 14 32 16 797  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3604236667.png)

2. 名称、密码、确认密码为必填项

![2024-05-31 14 33 44 318  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2467457849.png)

3. 一般情况下选择users用户组就可以，管理员可以选择administrators用户组。

![2024-05-31 14 36 06 408  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2481114087.png)

4. 共享文件夹权限可以根据实际情况给与。

![2024-05-31 14 37 21 742  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3803326887.png)

5. 设置空间配额，默认是无上限。

![2024-05-31 14 38 44 243  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/3281615326.png)

6. 根据实际情况给用户设置应用程序的权限。

![2024-05-31 14 41 18 813  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/1600095301.png)

7. 设置用户的速度限制，按提示完成及可创建新用户。

![2024-05-31 14 43 12 985  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/05/2271219295.png)

## 七、常用套件

桌面进入套件中心，根据自身需要安装如下套件：

1. Synology Drive server ——主要功能是同步文件，可以在多个设备之间共享文件，如电脑、手机和平板电脑，文章参考如下：

   [***群晖Drive客户端安装配置及日常使用管理_NAS存储***](https://post.smzdm.com/p/a5omwz58/)

2. Synology Photos ——自动备份手机里面的照片。官方教程如下：
   [***Synology Photos快速入门指南 - Synology 知识中心***](https://kb.synology.cn/zh-cn/DSM/tutorial/Quick_Start_Synology_Photos)

3. Cloud Sync ——同步NAS文件到百度网盘、Onedrive，异地备份的一种解决方案。

4. Container Manager ——群晖的Docker，在 DSM 上运行世界各地的开发人员创建的数干种容器应用程序

5. Synology office ——类似于金山文档，用于在 Synology Drive 中创建文档、电子表格和幻灯片。

6. Web Station ——可在安装 PHP 套件后轻松创建动态网站，以供个人和企业使用。

套件的种类很多，可以搜索网上相关教程或官方文档，特别的各类Docker应用。

## 八、提醒

1. 建议等熟练操作后再正式导入数据，且 重要数据多做备份（如定时移动硬盘冷备份，备份至阿里云盘等）。
2. 群晖的设置及应用内容较多，网上有很多大佬分享保姆级教程，请善用搜索引擎。



**作为文件存储、备份、共享的服务器**



**稳定运行最重要，不要随意更新系统**