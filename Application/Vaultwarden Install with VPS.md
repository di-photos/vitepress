# Vaultwarden安装注意事项

首先用SHH工具登录VPS后台。

## 一、安装Vaultwarden

```
docker run -d --name vaultwarden -v /vw-data/:/data/ -e SIGNUPS_ALLOWED=false -e ADMIN_TOKEN='****' --restart unless-stopped -p 18080:80 -p 1012:3012 vaultwarden/server:latest
```

上述ADMIN_TOKEN='************' 为管理页面登录密码，采用argon2加密，可以在VPS上安装argon2后运行如下命令获得（把“I am Password”改为需要的密码），且等号右侧需要有两个单引号（'）

```
echo -n "I am Password" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4
```



### 开放VPS的端口

```
sudo ufw allow 80/tcp

sudo ufw allow 443/tcp

sudo ufw allow 18080/tcp

sudo ufw allow 1012/tcp
```



### 迁移/恢复

场景：将数据从一台VPS迁至另一台VPS，或者VPS重装后，恢复之前的密码数据。

前提：备份/vw-data/目录的所有数据。按照上述搭建教程重新搭建，搭建完成后，删除/vw-data/目录下的所有文件，导入之前备份的文件，然后进入Docker管理器，重启容器，即可完成迁移/恢复。



## 二、配合Lucky做反向代理

1. 在cloudflare上把需要的二级域名进行解析到VPS的IP

2. 安装Lucky

    

   ```
   docker run -d --name lucky --restart=always --net=host -v /root/luckyconf:/goodluck gdy666/lucky
   ```

   \#挂载主机目录, 删除容器后配置不会丢失。可替换 /root/luckyconf 为主机目录, 配置文件为lucky.conf

   #默认登陆地址 : http://\{IP地址\}:16601
   #默认账号：666
   #默认密码：666

   提醒：登录后请在后台的“**设置**”中更改默认的账号及密码。

3. Lucky添加web服务

   ![](https://webp.nas-u.top/Nasu_241116133124.png)

   ![](https://webp.nas-u.top/Nasu_241116133431.png)

4. 添加SSL证书

   ![](https://webp.nas-u.top/Nasu_241116133839.png)

