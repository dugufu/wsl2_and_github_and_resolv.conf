## wsl2 （Ubuntu·）auto generate IP address 这个每次搞得我要直接去更改registry 永久修改resolv.conf文件配置
## wsl2 github unable to access的问题 ： 这里建议使用SSH 而不是HTTPS来clone project or push project 

<h1>那些wsl2 被啊三坑的洞 永久修改resolv.conf文件配置</h1>

每次都打开 powershell 输入 
$ wsl ~
等待进入
$ sudo nano /etc/resolv.conf
这里就会看到ip 又又被重置了 我这里是改好了的 如果没有处理的话
nameserver 192.168.120.1 
wsl开启的时候，随机生成一个

这里我去改文件，然后不再随机生成IP 我上网看了很多 像是resolvconf都试过了 ， 每次改了的文件，不能够永久替换

成品 ：
![image](https://user-images.githubusercontent.com/49250073/229793865-15be613c-aff9-427f-b744-4212bbf3e936.png)

## 记得 `IPv4 default gateway` and `IPv4 DNS Servers` 以下选一个方法来记得ip 等下会用
method 1 ：
打开Network connection 
选择你连接着的 Wifi or Ethernet 
右键 status
点击 details
记得 `IPv4 default gateway` and `IPv4 DNS Servers`
method 2 ：
在PowerShell run ipconfig /all
记得 `IPv4 default gateway` and `IPv4 DNS Servers`
---
然后在powershell 写 
wsl ~ <!-- 进入wsl（）Ubuntu）， 确定在root ， 方便一些新人看这篇 -->
sudo vi/nano/gedit “/etc/wsl.conf“<!-- vi/nano/gedit 选择其中一个写入器 推荐nano sudo nano /etc/wsl.conf -->
打开后的编辑器 写入
---
[network]
generateResolvConf = false

control+s Save control+x quit 保存退出
---
cd ~ <!-- 回去目录 ， 这里要删除文件 重新创建-->
sudo rm /etc/resolv.conf <!-- Remove document resolv.conf -->
cd /etc/ <!-- 进入etc -->
sudo touch resolv.conf <!-- 创建回resolv.conf -->
sudo nano /etc/resolv.conf <!-- 改写里面的文件 可以看下图知道IP来源 在powershell 写 ipconfig /all -->
nameserver 192.168.0.1 
nameserver 8.8.8.8
nameserver 8.8.4.4 <!-- 不一定要三个nameserver， 选择一个default ipv4 一个ipv4 DNS就可以了-->

然后 control+s control+x 保存退出
---
把上面记得/写入的 IPv4 default gateway & IPv4 DNS Servers 放进来 由于我的是google dns所以我是放了8.8.8.8 / 8.8.4.4 上网search 如何改成google dns如果你要的话， 不改就用自己的
![image](https://user-images.githubusercontent.com/49250073/229799862-800cb133-3d70-4f94-a701-d8757cd8ee02.png)
| 号码 |  解释           
|-----:|---------------
|     1|   这里我是用Eternet就是有线连接网络，如果是Wifi就找Wifi            
|     2|   这里是default gateway            
|     3|   这里是我的ipv4 dns 8.8.8.8 和 8.8.4.4            
---

然后restart 你的wsl
打开一个新的powershell 
wsl --shutdown
然后 wsl ~
ping google.com 看有一大推的ping test出来
sudo nano /etc/resolv.conf 看文件有没有 自己又自动生成
如果是 :
nameserver 192.168.0.1 
nameserver 8.8.8.8
nameserver 8.8.4.4 
恭喜你 成功了 !
