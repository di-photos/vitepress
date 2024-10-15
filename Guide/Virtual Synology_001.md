# 虚拟群晖——创建虚拟机及直通硬盘

**提醒：**

*本系列文章为Nas·U定制产品配套指引，若为自行组装机器仅供参考。内容包括IP获取、系统完善安装、用户创建、文件夹建立、基本设置、常用软件等相关内容。*

### 创建虚拟机

1. 官方下载涉及的三个文件，后续会用到。

- [**黑群晖引导文件（点击可到下载页面）**](https://github.com/RROrg/rr/releases)
- [**群晖SA6400最新69057版本系统（点击可下载）**](https://global.synologydownload.com/download/DSM/release/7.2.1/69057-1/DSM_SA6400_69057.pat?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057)

2. 把 黑群晖引导文件【rr.img】上传到isos文件夹

   ![2024-06-02 11 22 52 968  Gemoo Snap.png](https://webp.nas-u.top/3992246153.png)

3. 请在“虚拟机”标签页，找到群晖虚拟机左键弹出菜单选择“添加虚拟机”

   ![2024-06-02 11 25 52 177  Gemoo Snap.png](https://webp.nas-u.top/3667088107.png)

4. 选择Linux

   ![2024-06-02 11 30 04 758  Gemoo Snap.png](https://webp.nas-u.top/2900720861.png)

5. 按如下截图填写

   ![2024-06-02 11 34 58 378  Gemoo Snap.png](https://webp.nas-u.top/2757536767.png)

6. 主要虚拟磁盘位置选择 手动，并选择刚才上传到isos的引导文件【rr.img】,主要虚拟磁盘总线选择 USB

7. 点击主要虚拟磁盘位置下的“+”，增加磁盘

   ![2024-06-02 11 42 48 31  Gemoo Snap.png](https://webp.nas-u.top/223427880.png)

8. 若暂时想体验下，可以**增加虚拟硬盘**。

   ![2024-06-02 11 47 52 835  Gemoo Snap.png](https://webp.nas-u.top/4080860670.png)

   

### 直通硬盘

1. 若需要**直通硬盘**，磁盘位置选择“手动”，后面的列输入代码 /dev/disk/by-id/ata-【上述记下的硬盘IDENTIFICATION值】，例如：
   /dev/disk/by-id/ata-ST4000NM000A-2HZ100_WS315VTS，磁盘总线选择“SATA” 。

   *提醒：如果是拷贝修改，请确认这串代码中看是否有 **多余空格**，有空格会不成功*![2024-06-02 11 58 13 602  Gemoo Snap.png](https://webp.nas-u.top/2801767525.png)

2. 有多个硬盘直通给群晖就继续点左下角“+”，重复上述步骤增加磁盘。

3. 其他设置保持默认，取消勾选创建后启动虚拟机，点击创建。  

   ![2024-06-02 12 02 55 934  Gemoo Snap.png](https://webp.nas-u.top/1070169806.png)

   ![2024-06-02 12 04 39 52  Gemoo Snap.png](https://webp.nas-u.top/3275019141.png)

4. 黑群晖虚拟机创建完毕。  

5. 如若想要更高的磁盘性能，可以加装直通卡或者PCIE转SATA卡，并把 **SATA控制器直通** 给黑群晖。具体可参照网上相关教程，Nas·U后续也会更新SATA控制器直通相关教程。