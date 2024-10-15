# Unraid 系统下 WireGuard 设置指南

## 一、前提条件

Unraid 系统版本需在 6.8 及以上，安装 VPN 管理器后，才能进行 wireguard 设置。

首先，需要确保有公网 IPv4 或者公网 IPv6，这是实现远程访问的关键。公网 IP 的设置可能涉及到 DDNS 和防火墙或端口映射等操作。

拥有一个域名也非常重要，可通过多种方式获取，如免费的 duckdns。

只有满足这些前提条件，才能顺利进行后续的 wireguard 设置，实现安全、便捷的远程访问家庭设备。

## 二、Unraid 中 WireGuard 设置

![alt](undefined)

### （一）服务端配置

服务端配置是整个 WireGuard 设置的关键步骤之一。首先，自定义名称可以根据个人喜好或特定用途进行设置，比如命名为 “HomeServer”，方便识别和管理。在网络协议选择方面，要根据实际情况进行填写，一般情况下选择仅 IPv4 即可，除非有特殊需求才考虑同时启用 IPv6。

接下来，设置本地隧道网络池，例如设置为 “[10.253.0.0/24](http://10.253.0.0/24)”，这个网段不能与本地局域网段相同，以避免冲突。同时，为服务端分配一个虚拟局域网地址，如 “[10.253.0.1](http://10.253.0.1)”，同样不能与本地局域网相同。配置本地端点时，格式为公网绑定的域名和端口，比如 “[wg0.domain.com](https://wg0.domain.com:51820)[:5182](https://wg0.domain.com:51820)[0](https://wg0.domain.com:51820)”。另外，“Local server uses NAT” 一般设置为 “是”，本地隧道防火墙和 MTU 大小可默认留空。

### （二）对等节点配置

对等节点配置也是不可或缺的环节。设置对等节点名称，如 “RemoteAccess”，方便区分不同的对等节点。选择访问类型为 “Remote tunneled access”，确保能够实现远程隧道穿透。对等节点地址要与本地局域网 IP 不同，比如设置为 “[10.253.1.2](http://10.253.1.2)”。默认情况下，对等节点端点可不填，而允许的虚拟局域网 IP 可以设置为对等节点的地址，即 “[10.253.1.2](http://10.253.1.2)”。对等节点的 DNS 服务器可以默认与服务端 DNS 相同，可不填写。Persistent keepalive 可默认不填，若需要设置，可填写 20 - 25 秒，时间越长会增加对等节点客户端耗电量。

## 三、客户端配置

![alt](undefined)

### （一）不同平台客户端安装

1. **Windows 客户端安装**：首先在服务器端创建客户端配置文件，命名可根据个人喜好，比如 “PC”。下载到本地后，前往 WireGuard 官网（[https://www.wir](https://www.wireguard.com/)[egu](https://www.wireguard.com/)[ard](https://www.wireguard.com/)[.com/](https://www.wireguard.com/)）下载 Windows 客户端。官网地址清晰明确，方便用户快速找到下载入口。下载安装后打开客户端，选择 “新建隧道”，在选择窗口选定刚刚下载的客户端配置文件，点击 “连接” 即可完成 Windows 客户端的配置。

1. **手机客户端安装**：在服务器端创建手机端配置文件后打开二维码。然后在官网下载安卓或者 iOS 安装文件。对于 iOS 用户，可能需要美区账号，可先注册美区账号，在 app store 点击头像，滑到最下方，退出登录，再重新登录。安装好后，打开 WireGuard 手机客户端，点击 “+” 号，选择 “扫描二维码” 即可完成手机客户端的配置。

### （二）导入配置

无论是 Windows 客户端还是手机客户端，都可以通过扫码或导入隧道链接来激活配置。对于手机客户端，扫描二维码的方式非常便捷，直接将配置信息导入到客户端中。对于 Windows 客户端，选择导入隧道链接的方式也很简单，只需在客户端中找到相应的导入选项，选择下载好的配置文件即可。激活配置后，就可以实现通过 WireGuard 穿透 Unraid，享受安全、便捷的远程访问体验。同时，用户可以根据自己的需求进行进一步的设置和调整，以满足不同的使用场景。例如，可以设置 Persistent keepalive 的时间，根据不同平台的特点进行优化，以达到最佳的使用效果。

## 四、高级设置与故障排除

![alt](undefined)

### （一）高级设置

在 Unraid 下进行 WireGuard 设置时，有时可能需要进行一些高级设置以满足特定需求。例如，若有自定义 IP 地址的 Docker 或有严格网络要求的虚拟机，就需要对 WireGuard 进行特定更改。首先，在 WireGuard 隧道配置中，将 “使用 NAT” 设置为 “否”。接着，在路由器中添加一条静态路由，以爱快路由器设置为例，在网络设置 - 静态路由中添加目标地址为 “[10.253.0.0/24](http://10.253.0.0/24)”（子网掩码为 [255.255.255.0](http://255.255.255.0)），网关 / 下一跳网关设置为 Unraid 系统的 IP 地址，比如 “[192.168.1.100](http://192.168.1.100)”，优先级可根据实际情况设置，若路由器没有此选项则以实际为准。如果使用 pfSense，则还需要勾选 “静态路由过滤 - 绕过相同接口上流量防火墙规则” 的框。

此外，若有自定义 IP 地址的 Docker，需在 Docker 设置页面上将 “主机访问自定义网络” 设置为 “已启用”。这样可以确保 Unraid 服务器和 Docker、局域网的虚拟机和其他系统以及具有自定义 IP 的 Docker 在特定设置下都能正常访问。例如，当 “使用 NAT”= 否，并且 “主机访问自定义网络”= 启用，同时路由器已设置静态路由时，Unraid 服务器和 Docker（网络类型 bridge/host）、局域网的虚拟机和其他系统以及具有自定义 IP 的 Docker 都可访问。

对于 DNS 设置，若通过 IP 地址或完全限定域名（例如 [my.unraid.net](https://my.unraid.net)）访问设备，可将所需 DNS 服务器的 IP 地址添加到 “peer DNS 服务器” 字段中。可以使用连接到局域网的路由器的 IP 地址，或者像 [8.8.8.8](http://8.8.8.8) 这样的全球公用 DNS。

### （二）故障排除

当 WireGuard 无法连接时，可进行如下检查。首先，确认隧道在 Unraid 端和客户端都处于活动状态。需注意，“活动” 并不意味着 “已连接”，而只是意味着隧道从那一端开始。检查 DDNS 是否指向当前的公网 IP 地址，并分配给 “本地端点”。同时，确认通过路由器将正确的 UDP 端口转发到 Unraid，并将同一端口分配给 “本地端点”。

如果在设置客户端后对配置进行了任何更改，则需要重新设置客户端，以便它们具有最新的配置。请确保在按 “查看对等配置” 之前保存更改，否则二维码 / 配置文件不会更新数据。

对于第一个客户端，推荐先使用蜂窝数据连接（而非 wifi）进行 WireGuard 客户端配置。这可以排除与客户端网络相关的问题，并且二维码是传输设置的最简单方法。在手机上测试正常后，再转到其他客户端。

如果无法在客户端和服务器之间进行握手，请尝试做一些实际使用隧道的操作。客户端在开始连接之前可能正在等待流量。禁用客户端上的任何节能功能，尤其是手机在低功耗模式下可能无法正确使用 VPN。此外，可能需要禁用手机上的任何 “数据保护程序” 功能，以便 VPN 不受限制。

如果 “peer 连接类型” 包括其中一个 LAN 选项，但只能访问 Unraid，请转到 “设置”-“网络设置” 并查看 “启用桥接” 是否为 “是”。如果禁用桥接，将无法通过 WireGuard 访问 LAN。

如果通过 Internet 从另一个网络进行连接，请确保双方的网络使用不同的子网。例如，不能连接两个都使用 [192.168.1.0/24](http://192.168.1.0/24) 的网络。如果将 Cloudflare 用于 DDNS，请务必将 Cloudflare “代理状态” 配置为 “仅 DNS” 而不是 “已代理”。