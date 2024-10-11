# NAS应用 | 解决Unraid的 Docker 镜像无法下载问题

事情起因是近日在帮一位老哥远程安装docker，梯子都已经搭建好了，但无论如何都拉取不下来。一开始怀疑是梯子问题，后来在自己本机都无法成功，搜索了才知道是最近政策有变化，Docker镜像库失效。

**解决方式**：

1. Unraid 打开 **终端**

![2024-06-16 21 51 02 573  Gemoo Snap.png](https://pic.nas-u.top/2851442270.png)

1. 执行如下命令：

```
mkdir -p /etc/docker;
tee /etc/docker/daemon.json <<- EOF
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io"
    ]
}
EOF
```

![2024-06-16 22 05 17 705  Gemoo Snap.png](https://pic.nas-u.top/4273087831.png)

1. 需要重启Docker服务

- 标签页**设置** -> **Docker** 中先将 **启用 Docker** 设置为 **否**，点击应用。

![2024-06-16 22 39 23 283  Gemoo Snap.png](https://pic.nas-u.top/3255278056.png)
![2024-06-16 22 40 53 081  Gemoo Snap.png](https://pic.nas-u.top/2252878426.png)

- 之后再设置为**是**，再次点击应用，加速镜像地址即可生效。

![2024-06-16 22 44 25 741  Gemoo Snap.png](https://pic.nas-u.top/4103323832.png)

1. 重新安装Docke拉取镜像就可以正常拉取。

![2024-06-16 21 55 17 321  Gemoo Snap.png](https://pic.nas-u.top/4205518271.png)