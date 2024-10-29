# 群晖重置密码

白群晖上有 RESET 按钮，如果忘记密码，只要按住四秒钟直到哔声响将会重设管理者密码。

但黑群晖根本没有reset键，那忘记密码怎么办？这里分享两种解决办法。

## 一、方法1（适用于RR引导）

1. 连接显示器在引导盘引导页面选择“**Configure loader**”。

   ![](https://webp.nas-u.top/Nasu_241028180619.png)

2. 获取到RR引导界面的IP。

   ![](https://webp.nas-u.top/Nasu_241028181127.png)

3. 在其他设备输入该IP，及可进入RR引导设置界面，选择“**高级设置**”。

   ![](https://webp.nas-u.top/Nasu_241028181225.png)

4. 选择“**重置 DSM 系统密码**”。

   ![](https://webp.nas-u.top/Nasu_241028181311.png)

5. 选中需要修改密码的用户并更改。提示：admin管理员账户不能按此方法修改

   ![](https://webp.nas-u.top/Nasu_241028181350.png)

   ![](https://webp.nas-u.top/Nasu_241028181457.png)

   ![](https://webp.nas-u.top/Nasu_241028181556.png)

6. 退出返回上一层目录。

   ![](https://webp.nas-u.top/Nasu_241028181623.png)

7. 选择“**编译引导**”。

   ![](https://webp.nas-u.top/Nasu_241028181737.png)

8. 编译完成后选择“**启动**”，即可重启引导并进入后台，用新密码即可登录系统。

   ![](https://webp.nas-u.top/Nasu_241028181809.png)

   

## 二、方法2（万能法）

1. 关机把硬盘取出来，挂载到其它电脑上，用磁盘管理软件，比如Diskgenius挂载群晖硬盘分区，找到根目录下的/etc/shadow文件

2. 打开此文件，一行一个账户记录，每一行又被  **英文冒号：** 分隔成多个字段，找到admin管理的账户 ，第一个字段是账户名，第二个字段就是账户密码。该密码使用sha512哈希函数加密的，以目前计算能力是破不了的，只需要用已知的加密后密码修改这个字段即可。
   - 如果该系统有其他账户并道密码的，就把该账户第二个密码字段复制，然后覆盖admin管理员密码字段。并把这个修改后的shadow文件放回去。
   - 接上硬盘，开机输入密码就可以登陆管理员账户了，注意是账户的密码，别搞错了！
   - 每行的结构是这样的，只需要修改**第1个冒号**和**第2个冒号之间**的字符段，其它部分都不需要修改。admin:**$6$Wo0kPkgm$OAp0Wl2AsaE4n1y7qxB5Jns70Yk91AadfgvzElsR5addfGmoGCC8DUXkKzK7vyiV8wXNeaWNm861**:15832:0:99999:7:::
   - 如果没有其他账户，可以用已经加密好的密码（**123456789**） ，直接复制回去覆盖。成功登陆后再修改成你要的密码即可。**$6$n/atIskPWx$BTDud6vnw5pKQLcm4vHd3BEOBlh5ovSw3Iv9pMhW6BFWvCH5DNIIQFsqwfj1Oxi4WeexYfmHFPHHxyZ9XWImz1**

**提醒：**

一般修改nas第一块硬盘即可，如果不行需要修改nas所有的硬盘。

