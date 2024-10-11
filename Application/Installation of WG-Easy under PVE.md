# NAS应用 | PVE下安装WG-Easy


WG-Easy **最简单易用** 的高性能WireGuard服务器。可广泛应用于内网穿透、异地组网、远程办公等场景。本文仅在PVE平台下操作，Unraid、群辉等平台下可参照进行docker自定义部署。

## 一、前提

1. 需要有公网IP，若没有公网请安装zerotier或者Tailscale
2. 自备域名

## 二、安装方法

1. 利用PVE的shell，先安装curl库

   ```
   apt install curl -y
   ```

2. 使用脚本命令行安装docker环境，按提示选择即可

   ```
   bash <(curl -sSL https://linuxmirrors.cn/docker.sh)
   ```

3. 安装完成后，开启Docker服务和开机启动

   ```
   systemctl start docker
   systemctl enable docker
   ```

4. 查询是否成功配置好docker环境

   ```
   docker ps
   ```

5. 如果已经确认有docker环境，输入如下命令（部分设置需要根据实际情况修改，且不要漏了代码后面的“\”）

   ```
   docker run -d \
     --name=wg-easy \
     -e WG_HOST=XXXX.com \     ##域名或者服务器IP
     -e PASSWORD=Changeme001 \     ##个人密码,请自行更改
     -e WG_DEFAULT_ADDRESS=10.0.0.x \     ##后面的X表示为网络段，不能改为具体数值
     -e WG_DEFAULT_DNS=192.168.31.1 \     ##DNS服务器，若家中Nas建议改为主路由IP
     -e WG_ALLOWED_IPS=10.0.0.0/24,192.168.31.0/24 \     ##前为虚拟局域网段，后为家中局域网段
     -e WG_PERSISTENT_KEEPALIVE=25 \
     -v ~/.wg-easy:/etc/wireguard \
     -p 51820:51820/udp \
     -p 51821:51821/tcp \
     --cap-add=NET_ADMIN \
     --cap-add=SYS_MODULE \
     --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
     --sysctl="net.ipv4.ip_forward=1" \
     --restart unless-stopped \
     ghcr.io/wg-easy/wg-easy
   ```

6. 路由器做好51820、51821端口映射，具体每款路由器设置方式不同，请自行查找。

## 三、使用方法

1. 输入：PVE IP地址 + :51821，如`192.168.31.10:51821`可以看见后台界面，输入刚才设置的密码登录

![2024-06-02 15 19 22 526  Gemoo Snap.png](https://pic.nas-u.top/3322822774.png)

1. 点击右上角**+ New** 新增一个设备。
2. 访问[ **WireGuard官网** ](https://www.wireguard.com/install/),安装相应手机、电脑客户端。
3. 手机客户端：扫描设备列表右侧二维码。
4. 下载配置文件*.conf 并导入，电脑客户端左下角 **新建隧道** -- **从文件导入隧洞……**

![2024-06-02 15 45 16 399  Gemoo Snap.png](https://pic.nas-u.top/2749468657.png)

1. 选中导入的隧道，点连接，客户端即可与服务器进行虚拟组网，操作方式与局域网一致。

## 四、升级方法

1. 进入PVE的Shell进行如下操作：

   ```
   docker stop wg-easy
   docker rm wg-easy
   docker pull ghcr.io/wg-easy/wg-easy
   ```

2. 再按上述 **二、安装方法** 第5步在PVE下操作即可