# 物理群晖——获取群辉IP

**提醒：**

本系列文章为Nas·U定制产品配套指引，若为自行组装机器仅供参考。内容包括IP获取、系统完善安装、用户创建、文件夹建立、基本设置、常用软件等相关内容。

### 

1. U盘为已经制作好的引导U盘，请勿进行格式化。
2. 确认安装好硬盘并插上引导U盘，连接电源线、网线，开机。
3. 查找群晖有四种方法：

#### 方法一（更稳定，推荐）：

- 群辉官网下载群晖助手并安装

  https://global.synologydownload.com/download/Utility/Assistant/7.0.4-50051/Windows/synology-assistant-7.0.4-50051.exe?model=SA6400&bays=12&dsm_version=7.2.1&build_number=69057

- 打开群晖助手，一般情况下会自动显示局域网内群晖设备，如果没显示请点搜索，等显示群晖IP地址等相关信息后双击。

![2024-05-31 11 01 26 641  Gemoo Snap.png](https://webp.nas-u.top/2809347334.png)

#### 方法二：

直接在浏览器输入[http://find.synology.com](http://find.synology.com/)，会自动帮你查找NAS设备，点连接即可。

#### 方法三：

连接显示器，2分钟左右屏幕会显示设备IP，浏览器输入 "http://设备IP:5000"，如：[http://10.10.10.20:5000](http://10.10.10.20:5000/)

![2024-05-31 11 03 57 27  Gemoo Snap.png](https://webp.nas-u.top/2598487719.png)

#### 方法四：

进入路由器后台，找到新加入的设备IP，浏览器输入 "http://设备IP:5000"，如：[http://10.10.10.20:5000](http://10.10.10.20:5000/)