# NAS应用 | MoviePilot 安装与使用

> MoviePilot作者要求低调使用，请勿将本文转载至任何中国社交媒体与平台，包括但不限于小红书、Bilibili、微博、简书、什么值得买等。

相信绝大多数搭建过NAS的人都有一个比较重要的使用方式，那就是使用Emby、Plex、Jellyfin或者Infuse来搭建自己的家庭影院。在此之前，我一直在使用NasTools和Plex来完成我的家庭影院的建设，它可以很好地满足自动观影的需求。当我在NasTools中订阅某个电视剧或某个电影时，我不再关心资源下载和其他内容，而是等待NasTools自动完成，如下图所示。

![img](https://webp.nas-u.top/551232181.jpg)

后来，由于一些众所周知的因素，nastool的作者不得不停止更新。虽然最终的版本仍然可以维持我的日常需求，但有时不得不面对一些问题，比如搜索资源的时间越来越长。但好消息是，原来的NasTools作者最近又开了一个新口子，根据NasTools代码简化的要求，开发了一种新的影视管理工具[MoviePilot] (https://github.com/jxxghp/moviepilot)，看到别人描述的新版软件运行速度有了相当大的提升。
我也在这里记录了我的安装过程，在这里我使用Synology的容器管理器(以前的Docker管理器)，下面可以稍微修改一下以适用于其他Docker支持平台，这取决于你的发挥。

## Synology Nas 准备工作

### 安装 Container Manager

打开 Synology 套件中心搜寻 Container Manager 并进行安装，安装过程根据相应提示即可。

![img](https://webp.nas-u.top/248802261.jpg)

### 设置 Docker 工作目录

打开 Synology 控制面板，选择共享文件夹，创建新的共享文件夹，文件夹名称根据需求自行选择，我这里就叫做 docker。

![img](https://webp.nas-u.top/3159203661.jpg)

接下来，进入 Synology File Station，鼠标右键选择刚刚穿件的目录，进入 `属性` 菜单，选择 `权限` 菜单中的 `新增`,用户与组选项选择 `EveryOne` ，应用于全部文件夹，给予 `读取与写入` 权限，如下图:

![img](https://webp.nas-u.top/2917632130.jpg)

点击应用后再选择 `应用到这个文件夹，子文件夹及文件`，如下图，点击保存后即可。

![img](https://webp.nas-u.top/589933778.jpg)

### 设置项目运行目录开始创建项目

在 File Station 内进入上一步设置的目录，新建文件夹，根据个人喜好选择命名即可，我这里将命名为 `theater`。
设置完成后打开 Container Manager，点击左侧 `项目`，再选择 `新增`，根据自己喜好命名，路径选择为上一步设置的目录，来源选择 `创建 docker compose`，内容留空进入下一步即可，设置完成后如下图所示：

![img](https://webp.nas-u.top/1625359433.jpg)

完成后点进 `theater` 进入项目，选择 `YAML` 配置，我们的准备工作便已完成。

![img](https://webp.nas-u.top/3776558236.jpg)

我这里就继续沿用之前 NasTools 的配置文件了。

## MoviePilot 依赖项目准备

### CookieCloud 安装

根据 [MoviePilot](https://github.com/jxxghp/MoviePilot?ref=blog.digitalimmigrants.org) 项目描述，[CookieCloud](https://github.com/easychen/CookieCloud?ref=blog.digitalimmigrants.org) 为本项目的必须依赖，原文如下：

> 站点信息需要通过 CookieCloud 同步获取，因此需要安装 CookieCloud 插件，将浏览器中的站点 Cookie 数据同步到云端后再同步到 MoviePilot 使用。

#### 服务端 Docker 安装

尽管 MoviePilot 项目中已经包含了公共 CookieCloud 服务器，但是本着能自建即自建的原则，我们也会在这里自行搭建 CookieCloud 服务器，`docker compose` 代码如下：

```yaml
version: "3"
services:
  cookie-cloud:
    image: easychen/cookiecloud:latest
    restart: always
    network_mode: bridge
    hostname: cookie-cloud
    ports:
      - 8088:8088
123456789
```

将上面的代码复制粘贴进入 Container Manager，点击保存后选择启动，会看到 Container Manager 正在拉取镜像，如下图：

![img](https://webp.nas-u.top/1906151488.jpg)

等这里的代码结束运行后，在浏览器输入 `http://${Synology IP}:8088`，如果看到如下界面，则表示部署成功：

![CleanShot 2023-08-19 at 21.25.22](https://webp.nas-u.top/2371706953.png)

#### 浏览器插件安装

进入 [CookieCloud Release](https://github.com/easychen/CookieCloud/releases?ref=blog.digitalimmigrants.org) 下载最新的浏览器插件，并进行解压保存。进入 Chrome 插件设置：`chrome://extensions/`，打开开发者模式，选择 `加载已解压的扩展程序`，选择刚刚的解压目录，点击选择进行安装，如下图所示：

![CleanShot 2023-08-19 at 21.30.37](https://webp.nas-u.top/2410004911.png)

安装完成后，我们可以打开 CookieCloud 插件进行配置，具体的教程可以移步 [官方教程](https://github.com/easychen/CookieCloud?ref=blog.digitalimmigrants.org#官方教程) ，下面以我的配置进行示例：

![CleanShot 2023-08-19 at 21.40.37](https://webp.nas-u.top/3287402039.png)

设置完成后点击下方 `保存` 与 `测试` 确保这里配置正确。到此为止，CookieCloud 配置完成。

### 下载服务器安装（以 QBitTorrent 为例）

根据 MoviePilot 项目文档，其支持 `QBitTorrent` 与 `Transsisson`，大家可以根据个人喜好自行选择，我这里便以 QBitTorrent 为例，Transsisson 大家可以自行搜寻相关内容进行安装。

#### 服务端 Docker 安装

第一步我们需要在之前设置的 MovePilot 项目的运行目录内新建一个名为 `qbittorrent` 的目录，这里的目的是将 QBitTorrent 的部分文件映射出来，以便我们进行后续操作，如忘记密码的修改或者 docker 迁移。如下图所示：

![CleanShot 2023-08-19 at 21.51.42](https://webp.nas-u.top/658560687.png)

同时，我们需要新建 QBitTorrent 的下载目录，以我个人的设置为例，如下：

![CleanShot 2023-08-19 at 21.55.28](https://webp.nas-u.top/1348170087.png)

我这里设置为 `media/movies(seriese)/raw` 存储下载的原始媒体文件，`media/movies(seriese)/clean` 存储经过刮削的媒体文件，`downloads` 目录存储其他的下载内容。
以上内容准备后，我们便可以开始部署，`docker compose` 文件如下，你可以根据你的需求进行修改，以下仅为示例：

```yaml
version: "3"
services:
  qbittorrent:
    image: linuxserver/qbittorrent:latest
    container_name: qbittorrent
    network_mode: host # 可以更改为 bridge，我这里选择 host 仅为方便设置 ipv6
    restart: always
    volumes:
      - ./qbittorrent:/config   # 冒号左边请修改为你想保存配置的路径
      - /volume1/media:/media   # 媒体目录，多个目录需要分别映射进来，需要满足配置文件说明中的要求
      - /volume1/downloads:/downloads
    environment:
      - PUID=1026
      - PGID=100
      - WEBUI_PORT=8999
      - TZ=Asia/Shanghai  # 时区
12345678910111213141516
```

我们再次进入 Container Manager，选择 `停止` 项目，并将上文的代码复制粘贴，随后再次点击启动，如下图：

![CleanShot 2023-08-19 at 22.01.02](https://webp.nas-u.top/2629297845.png)

等待弹出窗口内代码运行完成后，浏览器内输入 `http:${Synology IP}:8999` 便可进入 QBitTorrent WEBUI，如下图：

![img](https://webp.nas-u.top/3006567014.jpg)

#### QBitTorrent WEBUI 配置

上一步后我们需要输入用户名（默认 admin）与密码（默认 adminadmin），便可以进入界面，点击设置图标便可以进入设置，各位根据自己的需求自行调参，可以参考 [qBittorrent 参数详细设置教程](https://blog.17lai.site/posts/f6b32521/?ref=blog.digitalimmigrants.org)。

![CleanShot 2023-08-19 at 22.11.59](https://webp.nas-u.top/3984575137.png)

### 媒体服务器安装（以 Plex 为例）

MovilePilot 支持 Emby、Plex 与 JsllyFin，大家可以根据个人需求选择相应的媒体服务器，我个人的体验结论是：

- Emby 提供了较好的多人分享体验，也提供了多种客户端，如 Android、iOS、macOS、Apple TV等多平台，但其在 Apple TV 端的 APP 体验截止本文书写日期仍旧比较糟糕，如需 Apple TV 使用，一般需要搭配 Infuse 使用。
- Plex 更加注重个人观影，也提供多平台客户端，Apple TV 客户端较为完善，达到了一般流媒体平台的流畅度与观影体验。
- JellyFin 没有过多体验，但是可以把它理解为免费开源版的 Emby，手头紧张的朋友可以选择。

#### 服务端 Docker 安装

在上一步中我们新建了媒体目录，在这一步便可以开始应用。同时，我们也需要新建一个目录用于存储 Plex 的配置，与上文中 QBitTorrent 相同，便不再赘述。这里的 `docker-compose` 文件如下，各位可以根据自己的需求进行修改：

```yaml
version: "3"
services:
  plex:
    image: linuxserver/plex:latest
    container_name: plex
    network_mode: host
    restart: always
    environment:
      - PUID=1026
      - PGID=100
      - VERSION=docker
      - TZ=Asia/Shanghai  # 时区
    volumes:
      - ./plex:/config
      - /volume1/media:/media
123456789101112131415
```

这里步骤如上文相同，复制粘贴重新构建项目即可，这里不再赘述。

#### Plex WEBUI 配置

经过上文配置，这时便可以打开浏览器输入 `http:${Synology IP}:32400`，按照文字说明进行配置，可以参考 [如何在 FreeNAS 上安装 Plex 媒体服务器](https://zh.farrautomation.com/how-install-plex-media-server-freenas?ref=blog.digitalimmigrants.org)，跳过前文的安装，直接从网页配置看起即可。

## 正文开始之 MoviePilot 安装

建议开始之前首先熟读 [MoviePilot 配置文档](https://github.com/jxxghp/MoviePilot?ref=blog.digitalimmigrants.org#配置)，熟悉每一项环境变量的命名与含义。

### 服务端 Docker 安装

#### 基础配置

与上一步相同的，我们需要新建一个目录用于存储 MoviePilot 配置，如图所示：

![CleanShot 2023-08-20 at 02.22.39](https://webp.nas-u.top/288371442.png)

然后我们便要开始稍显繁琐的 Docker 配置，我们跟随官方文档，开始一步步的编写 `docker compose` 文件，首先开头还是老几样：

```yaml
version: "3"
services:
  moviepilot:
    image: jxxghp/moviepilot:latest
    volumes:
      - ./movie-pilot:/moviepilot # 冒号左边请修改为你想保存配置的路径
      - ./movie-pilot/config:/config#  冒号左边请修改为你想保存配置的路径
      - /volume1/media:/media # 媒体目录，多个目录需要分别映射进来，需要满足配置文件说明中的要求
      - ./qbittorrent/qBittorrent/BT_backup:/BT_backup #qb 种子目录，转移和辅钟需要
      - ./nastools/config:/nas-tools/config # nt数据库，用于转移历史记录，如果之前没有安装过 NasTools 的可以忽略删除这一行
    restart: always
    network_mode: bridge
    ports:
      - 3003:3000
    hostname: movie-pilot
123456789101112131415
```

以上这一部分为基础配置，接下来才是重头戏。

#### 环境变量设置

要在 `compose` 文件中设置环境变量，其关键词为 `environment`，只需要在上文中的代码中继续加入即可，接下来我们来一个个的填入 需要配置的环境变量。

##### Docker 运行权限管理

首先我们要获取 Synology 用户的 `uid` 与 `gid`，这里可以通过 ssh 到 Synology 输入命令来获取，第一步需要打开 Synology 的 ssh 权限，如图所示：

![CleanShot 2023-08-20 at 02.37.02](https://webp.nas-u.top/1065484734.png)

接下来我们可以使用如下命令获取相关内容：

```bash
ssh ${username}@${Synology IP}
# 根据提示信任 Host 以及输入密码进行验证
username@SynologyNas:~$ id
uid=1026(username) gid=100(users) groups=100(users),101(administrators)
1234
```

获取到上述内容之后，我们便可以设置以下内容，关于这里为什么不设置默认值，因为默认值为 0 的话即表明该程序可以获得我们 Synology 的 root 权限，这是不大安全的，具体的大家可以自行搜寻相关资料进行了解，这里不再赘述，这一步我们需要添加的环境变量如下：

```yaml
    environment: 
      - PUID=1026 # 运行程序用户的 uid，默认 0
      - PGID=100  # 运行程序用户的 gid，默认 0
      - UMASK=022 # 掩码权限，默认 000，可以考虑设置为 022
      - MOVIEPILOT_AUTO_UPDATE=true #重启更新，true/false，默认 true
12345
```

##### Docker 网络以及用户管理

```yaml
    environment: 
      - MOVIEPILOT_CN_UPDATE=false #重启更新是否使用国内加速，true/false，默认false 
      - NGINX_PORT=3000 #WEB服务端口，默认3000，可自行修改，但不能为3001
      - SUPERUSER=admin #超级管理员用户名，默认admin，安装后使用该用户登录后台管理界面
      - SUPERUSER_PASSWORD=password  # 超级管理员初始密码，默认password，建议修改为复杂密码
      - API_TOKEN=moviepilot #API密钥，默认 moviepilot，在媒体服务器 Webhook、微信回调等地址配置中需要加上 ?token= 该值，建议修改为复杂字符串
      - PROXY_HOST=http(s)://ip:port #网络代理（可选），访问 themovied b需要使用代理访问，格式为 http(s)://ip:port
      - TMDB_API_DOMAIN=api.themoviedb.org # TMDB API地址，默认 api.themoviedb.org，也可配置为 api.tmdb.org 或其它中转代理服务地址，能连通即可
12345678
```

##### MoviePilot 文件下载路径管理

> DOWNLOAD_PATH： 下载保存目录，注意：需要将 moviepilot 及下载器的映射路径保持一致，否则会导致下载文件无法转移

这里的这一句话看起来有些绕，但是我们解决的方式其实很简单，我们只需要保持 MoviePilot 与 QBitTorrent 的 Docker 下载目录映射名称一致即可，我们在之前已经将其均设置为 `media`，所以这里无需过多考虑。

```yaml
    environment: 
      - DOWNLOAD_PATH=/media #下载保存目录，注意：需要将moviepilot及下载器的映射路径保持一致，否则会导致下载文件无法转移 - 仅供参考
      - DOWNLOAD_MOVIE_PATH=/media/movies/raw #电影下载保存目录，必须是DOWNLOAD_PATH的下级路径，不设置则下载到 DOWNLOAD_PATH
      - DOWNLOAD_TV_PATH=/media/series/raw #电视剧下载保存目录，必须是DOWNLOAD_PATH的下级路径，不设置则下载到 DOWNLOAD_PATH
      - DOWNLOAD_CATEGORY=false #下载二级分类开关，true/false，默认 false,开启后会根据配置 category.yaml 自动在下载目录下建立二级目录分类，因为我不需要这里进行更加详细的分类，电影与剧集两个分类即可，所以可以保持 false，各位可以根据自身需求进行更改
      - DOWNLOAD_SUBTITLE=true #下载站点字幕，true/false，默认true
      - REFRESH_MEDIASERVER=true #入库刷新媒体库，true/false，默认true
      - SCRAP_METADATA=true #刮削入库的媒体文件，true/false，默认true
      - TORRENT_TAG=MOVIEPILOT #种子标签，默认为 MOVIEPILOT，设置后只有 MoviePilot 添加的下载才会处理，留空所有下载器中的任务均会处理
123456789
```

##### MoviePilot 媒体库文件设置

这里的路径与上文相同，我们只需要保持 MoviePilot 与 Plex 的 Docker 媒体库目录映射名称一致即可。

```yaml
    environment:
      - LIBRARY_PATH=/media #媒体库目录，多个目录使用,分隔 - 仅供参考
      - LIBRARY_MOVIE_NAME=movies/clean #电影媒体库目录名，默认电影
      - LIBRARY_TV_NAME=series/clean #电视剧媒体库目录名，默认电视剧
      - LIBRARY_CATEGORY=false # 媒体库二级分类开关，true/false，默认 false，开启后会根据配置 category.yaml 自动在媒体库目录下建立二级目录分类，可以根据自身需求进行更改
# 转移方式，支持link/copy/move/softlink
      - TRANSFER_TYPE=link
1234567
```

##### CookieCloud 配置

```yaml
    environment:
      - COOKIECLOUD_HOST=http://10.0.0.6:8088  # CookieCloud服务器地址，格式：http://${Synology IP}:port，必须配置，否则无法添加站点
      - COOKIECLOUD_KEY=从 CookieCloud 插件复制
      - COOKIECLOUD_PASSWORD=从 CookieCloud 插件复制
      - COOKIECLOUD_INTERVAL=180  # CookieCloud同步间隔（分钟）
# CookieCloud对应的浏览器UA
      - USER_AGENT=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36 #CookieCloud 对应的浏览器 UA，可选，设置后可增加连接站点的成功率，同步站点后可以在管理界面中修改
1234567
```

##### 通知器设置

暂时跳过。

##### 下载器配置

以 QBitTorrent 为例，其他的请自行探索。

```yaml
    environment:
      - DOWNLOADER=qbittorrent #下载器，支持qbittorrent/transmission，QB 版本号要求>= 4.3.9，TR 版本号要求 >= 3.0，同时还需要配置对应渠道的环境变量，非对应渠道的变量可删除，推荐使用 qbittorrent
#qbittorrent设置项
      - QB_HOST=http://10.0.0.6:8999 #qbittorrent地址，格式：http://${Synology IP}:port，https需要添加 https:// 前缀
      - QB_USER=username #qbittorrent用户名
      - QB_PASSWORD=passport #qbittorrent密码
      - DOWNLOADER_MONITOR=true #下载器监控，true/false，默认为true，开启后下载完成时才会自动整理入库
1234567
```

##### 媒体服务器设置

以 Plex 为例，首先我们需要获取 `X-Plex-Token`，在浏览器中登陆 Plex，`F12` 进入开发者模式，选择 `Network`，搜索栏输入关键词进行搜寻，如下图所示：

![CleanShot 2023-08-20 at 03.21.19](https://webp.nas-u.top/592152975.png)

获取后进行复制粘贴即可。

```yaml
    environment:
      - MEDIASERVER=plex #媒体服务器，支持emby/jellyfin/plex，同时还需要配置对应媒体服务器的环境变量，非对应媒体服务器的变量可删除，推荐使用emby
      - PLEX_HOST=http://10.0.0.6:32400 # Plex 服务器地址，格式：http://${Synology IP}:port，https 需要添加 https:// 前缀
      - PLEX_TOKEN= # Plex 网页 Url 中的 X-Plex-Token ，通过浏览器F12->网络从请求URL中获取
      - MEDIASERVER_SYNC_INTERVAL=1 #媒体服务器同步间隔（小时），默认 6，留空则不同步
12345
```

##### 认证站点设置

因为某些原因，MoviePilot 首先需要你时某一些 PT 站点的会员，才可进行使用，尽管这样提高了本项目的使用门槛，但这样可以使得本项目更加长久。详细内容可以参考 [Movie Pilot 用户认证](https://github.com/jxxghp/MoviePilot?ref=blog.digitalimmigrants.org#2-用户认证)。
这里以 `Audiences` 进行举例，我们首先需要获取 `uid` 与 `passkey`，登陆网站后，点击控制面板，即可获得，如下图所示：

![CleanShot 2023-08-20 at 03.28.18](https://webp.nas-u.top/16619216.png)

获取后进行复制粘贴即可。

```yaml
    environment:
      - AUTH_SITE=audiences  # 认证站点，支持hhclub/audiences/hddolby/zmpt/freefarm/hdfans/wintersakura/leaves/1ptba/icc2022/iyuu
      - AUDIENCES_UID=uid
      - AUDIENCES_PASSKEY=passkey
1234
```

#### 部署 MoviePilot

与上文相同的，将最后的完成版复制粘贴加入 Container Manager，重新构建项目即可。

### WEBUI 配置

打开 `http://${Synology IP}:3003`，如果出现登录界面，即表明安装成功，如下图所示：

![img](https://webp.nas-u.top/1675228077.jpg)

输入上文中设置的账号密码登录后，即可进入主界面，如下图所示：

![img](https://webp.nas-u.top/1710848352.jpg)

接下来更加详细的关于 WEBUI 内的配置就让你来进行探索了。