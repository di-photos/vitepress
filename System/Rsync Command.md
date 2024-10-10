# **文件传输Rsync命令**

在 Unraid 中，`rsync` 命令最常被作为文件传输工具所使用，了解并熟悉使用 rsync 命令可以让你轻松的在 Unraid 上实现文件的拷贝、转移和同步。

## 1-介绍

Rysnc 通过对比文件的修改时间和文件大小，来实现本地或本地与远程服务器之间的文件传输与同步，常用于文件备份和文件镜像。

> 它可以在本地计算机与远程计算机之间，或者两个本地目录之间同步文件（但不支持两台远程计算机之间的同步）。它也可以当作文件复制工具，替代 `cp` 和 `mv` 命令。 它名称里面的 `r` 指的是 remote，rsync 其实就是"远程同步"（remote sync）的意思。与其他文件传输工具（如 FTP 或 scp）不同，rsync 的最大特点是会检查发送方和接收方已有的文件，仅传输有变动的部分（默认规则是文件大小或修改时间有变动）。

它具备以下优点：

- **远程传输**：可以实现本地与远程主机的文件传输与同步。

- **灵活强大**：具备多种控制参数，可以灵活控制文件传输的各种行为。

- **增量同步**：利用 delta-transfer 算法可以实现文件之间只传输文件不同的部分，相同的文件不会重复传输。

- **断点传输**：即使传输中断了，再次传输时依然会从上一次中断的地方开始而不是重新传输。

- **加密传输**：支持匿名传输和认证传输，保证传输的安全性。

## 2-使用方法

`rsync` 命令的参数很多，但是大多数情况下我们在 Unraid 上只需要用常用的那几个即可，常用的命令格式和参数说明如下：

```bash
rsync -avhzP --append source/ destination
```

- **`-a`** ：必填参数。表示递归，即包含子目录一起进行传输，除此之外 `-a` 参数还可以同步元信息（比如修改时间、权限等）。

- **`-v`** ：将结果输出到终端，这样就可以看到哪些内容会被同步（ `-vv` 表示输出更详细的信息，`-vvv` 表示输出最详细的信息）。

- **`-h`** ：参数表示以人类可读的格式输出，可以配合 `-v` 一起使用来更好的查看传输过程中哪些数据被进行传送。

- **`-z`** ：传输时压缩数据。

- **`-P`** ：此参数是 `--progress` 和 `--partial` 这两个参数的结合：

- - `--progress` ：表示显示进展，即传输某个文件的时候会有一个进度条展示。
  - `--partial` ：允许恢复中断的传输。不使用该参数时，`rsync` 会删除传输到一半被打断的文件；使用该参数后，传输到一半的文件也会同步到目标目录，下次同步时再恢复中断的传输。一般需要与 `--append` 或 `--append-verify` 配合使用。

- **`--append`** ：指定文件接着上次中断的地方，继续传输。你也可以使用 `--append-verify` 进行替换，表示会对传输完成后的文件进行一次校验，如果校验失败，将重新发送整个文件。

- **`source/`** ：表示将 `源目录` 下的所有文件进行传输，但不包含 `源目录` 本身。

- **`destination/`** ：表示文件存放的目的地目录，即将所有要传输的文件存放到 `目标目录` 下。

**其他常用参数参考：**

- `--exclude` ：参数指定排除不进行同步的文件，例如 `--exclude="*.zip"` 。

- `-n` （ `--dry-run` ）：模拟将要执行的操作，而并不真的执行，配合 `-v` 参数使用，可以看到哪些内容将会被同步过去。

- `--size-only` ：表示只同步大小有变化的文件，不考虑文件修改时间的差异。

- `--max-size` ：设置传输的最大文件的大小限制，比如不超过200KB `--max-size='2000k'` 。

- `--min-size` ：设置传输的最小文件的大小限制，比如不小于10KB `--min-size=5k `。

- `--remove-source-files` ：表示传输成功后，删除发送方的文件。

## 3-应用案例

大多数情况下为了使用方便，不管是传输单文件还是传输文件夹及其里面的所有文件，都可以添加上 `-avhzP --append` 这几个参数，因此下文中的这些示例中也都是这么去使用。

### 3.1-单文件拷贝

将名为 `Test01` 共享文件夹内的 `Bandicam-7.1.2.2451-x64-Portable.exe` 文件拷贝到 `Test02` 目录下：

```bash
rsync -avhzP --append /mnt/user/Test01/Bandicam-7.1.2.2451-x64-Portable.exe /mnt/user/Test02/
```

### 3.2-多文件拷贝

将 `Text1.txt` 和 `Text2.txt` 同时拷贝到 `/mnt/user/Test02/` 文件夹目录下：

```bash
rsync -avhzP --append /mnt/user/Test01/Text1.txt /mnt/user/logs/Text2.txt /mnt/user/Test02/
```

### 3.3-文件夹及内部文件拷贝（递归拷贝）

如果需要将文件夹自身整体进行转移，那么不需要在文件夹路径最后添加斜杠 `/` ；如果只是需要将文件夹内的文件进行转移，那么请加上 `/` 。

将 `/mnt/user/Test01/File01` 文件夹内的所有文件都拷贝到 `/mnt/user/Test02` 共享文件夹下，但不包含 `File01` 文件夹自身：

```bash
rsync -avhzP --append /mnt/user/Test01/File01/ /mnt/user/Test02/
```

基于上述的例子，如果需要将整个 `File01` 文件夹都进行转移，则需要去掉 `File01` 文件夹后面的斜杠符号 `/` ：

```bash
rsync -avhzP --append /mnt/user/Test01/File01 /mnt/user/Test02/
```

也就是将 `File01` 文件夹转移到 `Test02` 文件夹下

### 3.4-剪切拷贝

实现剪切的效果需要使用 `--remove-source-files` 参数，此参数的作用是当文件拷贝完成后将源文件删除，从而实现剪切的效果。

例如下面命令中将 `Bandicam-7.1.2.2451-x64-Portable.exe` 传输完成后，此文件就会被删除（从效果上来说就相当于将文件进行了剪切）：

```bash
rsync -avhzP --append --remove-source-files /mnt/user/Test01/Bandicam-7.1.2.2451-x64-Portable.exe /mnt/user/Test02/
```

### 3.5-远程同步

`rsync` 支持远程与远程服务器之间进行文件传输，传输可以使用 SSH 协议也可以使用 `rsync` 协议（前提是远程服务器也需要安装 rsync 命令）。本文只介绍使用 SSH 协议进行远程传输的方法，使用 `rsync` 协议的方法可以参考文章末尾的参考文章。

为了简化，这里假设远程服务器也是一台 Unraid ，SSH 端口为 22 。

#### 3.5.1-本地传输文件到远程服务器

将本地 `/mnt/user/Test01/Bandicam-7.1.2.2451-x64-Portable.exe` 文件传输到远程 `10.10.10.10` 的 Unraid 服务器上，通过 SSH 协议使用 `root` 用户进行登录之后，将文件传输到远程服务器下的 `/mnt/user/Test03/` 目录：

```bash
rsync -avhzP --append /mnt/user/Test01/Bandicam-7.1.2.2451-x64-Portable.exe root@10.10.10.10:/mnt/user/Test03/
```

敲下命令之后会要求你输入远程服务器 `root` 用户的密码

#### 3.5.2-远程服务器传输文件到本地

将远程 Unraid 服务器（ `10.10.10.10` ）下的 `/mnt/user/Test03/Bandicam-7.1.2.2451-x64-Portable.exe ` 文件传输到本地服务器的 `/mnt/user/Test01/` 目录下

```bash
rsync -avhzP --append root@10.10.10.10:/mnt/user/Test03/Bandicam-7.1.2.2451-x64-Portable.exe /mnt/user/Test01/
```

敲下命令之后会要求你输入远程服务器 `root` 用户的密码

#### 3.5.3-补充说明：非 22 端口的 SSH

前面的命令是基于远程服务器 SSH 服务使用的是默认的 22 端口，因此使用 SSH 连接时不需要指定端口号，但是如果远程服务器的 SSH 服务使用的不是 22 端口，那么需要在 rsync 命令中使用 `-e` 参数。

> `-e` 参数的作用是指定所要执行的 SSH 命令，例如 `-e 'ssh -p 567'` 表示使用 567 端口进行 SSH 登录。

例如下面的命令表示使用 `-e` 参数使用 SSH 的 567 端口进行登录并传输文件：

```bash
rsync -avhzP --append -e 'ssh -p 567' /mnt/user/Test01/Bandicam-7.1.2.2451-x64-Portable.exe root@10.10.10.10:/mnt/user/Test03/
```

#### 3.5.4-使用 rsync 命令与群晖服务器进行文件传输

在控制面板中开启 rsync 文件服务：

![4084fca939c39a919dda0.png](https://img.nas-u.top/file/4084fca939c39a919dda0.png)

然后使用以下命令将群晖的 `Jianying6.01.rar` 文件传输到本地 Unraid 上的 `Test01` 文件夹：

> 将本地文件传输到群晖也是一样的使用方式。

![ab9ca0f237ecca1fa255b.png](https://img.nas-u.top/file/ab9ca0f237ecca1fa255b.png)

获取群晖上文件的详细存储地址，然后放到命令里面

```bash
rsync -avhzP --append user@10.10.10.4:/volume3/Temp/04 软件/Jianying/Jianying6.01.rar /mnt/user/Test01/
```

`user` 是登录到群晖的账号，`10.10.10.4` 是群晖的内网地址

## 4-补充说明

### 4.1-**Dynamix File Manager**

对于文件的拷贝和转移，很多时候我们通过 WebUI 界面就可以实现简单的文件传输，例如你可以在 Unraid 上安装一个叫做 **Dynamix File Manager** 的插件之后即可实现 WebUI 页面的文件管理 —— 增删改查等：

![f4a1b10a178ed7030f4b2.png](https://img.nas-u.top/file/f4a1b10a178ed7030f4b2.png)

安装好插件之后即可在“共享”界面,，点击目录最右侧的“查看”下部的按钮实现文件的增删改查

![a32333c39764e55e9513e.png](https://img.nas-u.top/file/a32333c39764e55e9513e.png)

![ef046b2b09244fc15eaa6.png](https://img.nas-u.top/file/ef046b2b09244fc15eaa6.png)

![28b3123179ecdda454b97.png](https://img.nas-u.top/file/28b3123179ecdda454b97.png)

本质上，此插件也一样会在文件拷贝或者转移时根据实际情况去使用 `rsync` 命令，所以在 Unraid 上 `rsync` 是一个很通用的文件拷贝和同步工具，理解并且熟练的运用可以很方便的去管理文件。