# 群晖激活Active Backup for Business 套件

通过 Synology Active Backup for Business 软件，可以在单一界面上集中管理分散在 PC、服务器、虚拟机中的数据，并可在灾难发生时，在服务主机或 NAS 上实时重启服务，保障业务的连续性。白群用户可以直接激活，通过QC连接使用。

对于黑群用户来说，需要注意以下几点并进行激活：

1. 密码中建议**只包含字母和数字**，如果存在特殊符号，建议修改一下密码再操作。

2. 不开启双重验证。

3. 只建议使用[火狐浏览器](http://www.firefox.com.cn/)激活，其它浏览器激活成功概率低。

4. 在套件中心安装好Active Backup for Business套件。

5. 到【控制面板】 - 【信息中心】 找到【产品序列号】并记下来。

------

**再次提醒：请使用[火狐浏览器](http://www.firefox.com.cn/)操作**，在浏览器地址栏中分别输入以下网址，替换地址内的ip地址端口和用户名密码sn信息。

**DSM6.x系统版本**

把文本复制到记事本里面然后打开两个页面分别输入修改好的地址，然后两个页面来回刷新。

```
https://群晖ip地址:https端口号/webapi/auth.cgi?api=SYNO.API.Auth&method=Login&version=1&account=群晖本地用户名&passwd=群晖本地密码
https://群晖ip地址:https端口号/webapi/entry.cgi?api=SYNO.ActiveBackup.Activation&method=set&version=1&activated=true&serial_number="群晖的SN码"
```

**DSM7.x系统版本**

把文本复制到记事本里面然后打开两个页面分别输入修改好的地址，然后两个页面来回刷新。

```
https://群晖ip地址:https端口号/webapi/auth.cgi?api=SYNO.API.Auth&version=3&method=login&account=管理员用户名&passwd=密码&format= cookie
```

注意：**cookie前面有英文空格**，网页提示中有"success":ture字样就是OK了。

```
https://群晖ip地址:https端口号/webapi/entry.cgi?api=SYNO.ActiveBackup.Activation&method=set&version=1&activated=true&serial_number="序列号"
```

网页提示success和activated同时为true时说明激活成功。