# NAS应用 | Unraid部署的思源笔记不能启动问题

思源笔记部署好一直稳定使用，最近发现该容器没启动，查看docker日志，发现文件夹无权限，回想最近操作应该与前两天对Unraid部分文件夹设置【新权限】有关。

![image-20240419113059-bjda9kk.png](https://pic.nas-u.top/220304459.png)

**解决办法**：

进入Unraid命令行模式，输入

```
 chown -R 1000:1000  /mnt/user/appdata/siyuan
```

‍*上述路径为容器设置中的/siyuan/workspace的宿主机路径*