# NAS使用指引 | Unrai及虚拟群晖

本文为博主定制产品配套指引，若为自行组装机器仅供参考。内容包括IP获取、系统完善安装、用户创建、文件夹建立、基本设置、常用软件等相关内容。

## 一、前期准备（获取直通给群晖的硬盘ID，避免错乱）

1. 确认U盘引导盘已经插入后面主板USB接口，并把要直通给黑群晖的硬盘（数量自定，建议并放在同一侧，并贴上标签纸）插入盘位，其他需要分配给unraid的暂时不插。接通网线，接上电源并开机。
2. 开机3分钟左右，查找unraid的后台IP，有两种方法：

- #### 方法一（推荐）：

  进入路由器后台（每个品牌的路由器后台进入方式不同，请根据自家情况而定），找到全部接入网络所有的设备，通常最近接入的设备（显示名称为nas）一般就是Nas设备。

- #### 方法二：
  
  连接显示器，最后出现login字样时，前几行的IPV4地址即为该设备IP地址
  
  ![2024-06-01 12 53 26 681  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1268169739.png)

3. 浏览器中输入该设备的IP，即可显示后台登录界面，如下图。输入默认用户名：**root** 密码: **Changeme001** （C为大写，其他为小写） 即可登录后台。

   ![2024-06-01 12 59 13 056  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2458548066.png)

​     ***若不使用黑群晖，可省略本节4~6步骤，直接到第二节。***

4. 点击 “主界面”标签，一般情况下会显示接入的硬盘信息，记下“未分配设备”中的“IDENTIFICATION”信息，直通给黑群晖用。

   ![2024-06-01 13 05 29 233  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1072824289.png)

5. 右上角点关机，一般2分钟左右关机完毕。

   ![2024-06-01 13 08 51 388  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/4229912516.png)

6. 把剩余分配给unraid的硬盘插入盘位，重新开机进入unraid后台。

## 二、给Unraid分配硬盘

1. 点击“主界面”标签页，可看到所有插入的硬盘信息。**若阵列已经启动**请按如下第2部操作，若未启动直接到第3步。

2. 点击下方的“阵列操作”中的“停止”按钮，大概1分钟后可全部停止。

   ![2024-06-01 13 16 55 118  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2962009447.png)

3. 停止阵列后，“阵列设备”奇偶校验可以不选。在下方的磁盘1~磁盘10添加硬盘到阵列中，提醒：把给unraid的硬盘进行分配（可多个），若需直通给黑群晖的硬盘不要在此分配。

   ![2024-06-01 13 30 57 034  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/626270220.png)

4. 点击下方的“阵列操作”中的“启动”按钮，启动阵列。

   ![2024-06-01 13 34 00 991  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2674804747.png)

5. 由于新增硬盘不是UNRAID格式，需要格式化，页面下部会有格式化的提示，请按照下图操作，打勾，然后点击格式化

   ![2024-06-01 13 39 46 326  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3725604408.png)

6. 格式化以后，硬盘的空间就可以在上方的“阵列设备”看到了。

7. 设置磁盘自启动：在设置标签页，点击磁盘设置。

   ![2024-06-01 13 46 13 346  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2446733283.png)

8. 按下图设置启用自动启动、默认休眠延迟项，点应用。

   ![2024-06-01 13 48 54 689  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2508942751.png)

9. 后续若需添加硬盘，请按 **第2步** 提示停止阵列。

10. 点击“工具”标签页下的“新配置”，保留当前分配项选择“无”，勾选“是的，……”再点“应用”

       ![2024-06-01 14 18 44 504  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2615495887.png)

       ![2024-06-01 14 22 00 554  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1071691398.png)

11. 按第3步的提示重新添加阵列设备即可，原有数据不受影响。

## 三、用户设置

1. 因windows特性问题，SMB共享**不能用root用户名登录**，所以前期已经增加了user用户，用户名：**user** 密码：**Changeme001**（C为大写，其他为小写）。
2. 可以在用户标签自行更改密码，提醒：用户密码要求最前面的密码为大写

## 四、创建共享文件夹

1. 点击“共享”标签页”添加共享”

   ![2024-06-01 14 29 15 164  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1339544430.png)

2. 填写共享文件夹的名称，选择共享文件夹存放那个盘，点添加共享按钮

   ![2024-06-01 14 35 43 236  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/65836156.png)

3. 在弹出来的“SMB 安全设置”的导出选项中选“是”，再点应用按钮。

   ![2024-06-01 14 40 25 983  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2258051620.png)

4. 创建了共享文件夹后，打开我的电脑的地址栏，输入`\\ NAS IP地址`。如`\\10.10.10.10`，在弹出的对话框输入用户名：user 密码：Changeme001（C为大写，其他为小写），可访问NAS共享文件夹。
   
   ***关联：下列安装黑群晖步骤中会上传文件到isos文件夹，请参考该步骤\***
   
   ![2024-06-01 14 45 36 072  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/243211156.png)

5. 常用文件夹可以右键点击 映射网络驱动器，创建完成就可以当成本地磁盘用。

## 五、Unraid常用Docker简介

1. Heimdall——自行添加网址作为导航页，免除每次输入IP地址及端口，方便管理Nas各种服务。设置简单，网上保姆级教程很多，请善用搜索引擎。
2. Mt-Photos——个人认为Docker最好的照片管理系统。因为该软件实行订阅制，若使用良好，请支持正版软件。
   *另外的替代方案就是黑群晖当中的照片，在群晖的套件中安装即可。*
3. Filebrowser——很好用的文件管理器，轻松对Nas中的文件进行复制、删除、下载等操作。
4. Jellyfin——开源的影视媒体管理系统，Emby在闭源后的分支，使用体验与Emby差不多。用户名：
5. Qbittorrent——一款非常优秀的PT下载工具，由于其开放性和易玩性，是玩PT的首选。
6. 常见的几个Docker安装模板已经放置在U盘中，按如下提示可以直接使用
7. 更多的设置，请参考“阿文菌”大佬的教程：
   https://post.smzdm.com/p/aoow5ml7/

*提醒：unraid系统的设置已基本设置妥当，涉及Docker、虚拟机的内容较多，请根据自身情况选择安装，网上保姆级教程很多，请善用搜索引擎。*

## 六、黑群晖直通硬盘及配置引导

1. 官方下载涉及的三个文件，后续会用到。

- [**黑群晖引导文件（点击可下载）**](https://objects.githubusercontent.com/github-production-release-asset-2e65be/625183383/6d9a7ae9-ce73-47e2-981e-3d2bfedc75a0?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20240522%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240522T004247Z&X-Amz-Expires=300&X-Amz-Signature=8f7c960981dc270caf064b688f006b50be0f382eecdfd50dab901a72782add6c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=625183383&response-content-disposition=attachment%3B filename%3Drr-24.5.5.img.zip&response-content-type=application%2Foctet-stream)
- [**群晖SA6400最新69057版本系统（点击可下载）**](https://global.synologydownload.com/download/DSM/release/7.2.1/69057-1/DSM_SA6400_69057.pat?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057)

2. 把 黑群晖引导文件【rr.img】上传到isos文件夹

   ![2024-06-02 11 22 52 968  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3992246153.png)

3. 请在“虚拟机”标签页，找到群晖虚拟机左键弹出菜单选择“添加虚拟机”

   ![2024-06-02 11 25 52 177  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3667088107.png)

4. 选择Linux

   ![2024-06-02 11 30 04 758  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2900720861.png)

5. 按如下截图填写

   ![2024-06-02 11 34 58 378  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2757536767.png)

6. 主要虚拟磁盘位置选择 手动，并选择刚才上传到isos的引导文件【rr.img】,主要虚拟磁盘总线选择 USB

7. 点击主要虚拟磁盘位置下的“+”，增加磁盘

   ![2024-06-02 11 42 48 31  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/223427880.png)

8. 若暂时想体验下，可以**增加虚拟硬盘**。

   ![2024-06-02 11 47 52 835  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/4080860670.png)

9. 若需要**直通硬盘**，磁盘位置选择“手动”，后面的列输入代码 /dev/disk/by-id/ata-【上述记下的硬盘IDENTIFICATION值】，例如：
   /dev/disk/by-id/ata-ST4000NM000A-2HZ100_WS315VTS，磁盘总线选择“SATA” 。

   *提醒：如果是拷贝修改，请确认这串代码中看是否有 **多余空格**，有空格会不成功*

   ![2024-06-02 11 58 13 602  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2801767525.png)

10. 有多个硬盘直通给群晖就继续点左下角“+”，重复上述步骤增加磁盘。

11. 其他设置保持默认，取消勾选创建后启动虚拟机，点击创建。   ![2024-06-02 12 02 55 934  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1070169806.png)

      ![2024-06-02 12 04 39 52  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3275019141.png)

12. 点击刚才创建的虚拟机，弹出菜单选择“Start with ……”项启动。 ![2024-06-02 12 07 37 646  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3964055645.png)

13. 会弹出页面自动跑代码，待出现下列IP时，在电脑浏览器输入该IP，后续就可通过浏览器操作了   

    ![2024-06-02 12 21 30 209  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3339777353.png)

14. 更改为中文

      ![2024-06-02 12 24 42 64  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/4121746216.png)

      ![2024-06-02 12 25 57 447  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3137554071.png)

15. 选择型号为SA6400

      ![2024-06-02 12 28 03 788  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2934947846.png)

       ![2024-06-02 12 29 09 233  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2275072233.png)

16. 选择版本，进去后选7.2（最新版本）,在确认页面回车确认即可

       ![2024-06-02 12 31 21 208  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3650055912.png)

       ![2024-06-02 12 32 01 465  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1893203826.png)

       ![2024-06-02 12 34 23 945  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/596832023.png)

17. 选择编译引导后自动跑代码

       ![2024-06-02 12 36 20 84  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2072264305.png)

       ![2024-06-02 12 36 42 965  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2792127127.png)

18. 跑完代码自动回到主页面，选择 启动，并弹出如下页面。

       ![2024-06-02 12 39 32 494  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/1044748778.png)

       ![2024-06-02 12 39 59 797  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/3876941721.png)

19. 等待2分钟左右，按上面的IP，如上述截图的10.10.10.39:5000，也可利用[ **群晖助手(可点击下载)** ](https://global.synologydownload.com/download/Utility/Assistant/7.0.4-50051/Windows/synology-assistant-7.0.4-50051.exe?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057)搜索局域网内的群晖主机，双击可进入群晖安装界面。

       ![2024-06-02 12 41 56 302  Gemoo Snap.png](https://nas-u.top/usr/uploads/2024/06/2337385190.png)

## 七、群晖安装事宜详见 上一篇章 “物理群晖”