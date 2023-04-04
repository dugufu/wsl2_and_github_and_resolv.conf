 ### 第一个问题 :wsl2 （Ubuntu·）auto generate IP address 这个每次搞得我要直接去更改registry 永久修改resolv.conf文件配置
 ### 第二个问题 :wsl2 github unable to access的问题 ： 这里建议使用SSH 而不是HTTPS来clone project or push project 

#第一个问题
<h1>那些wsl2 被啊三坑的洞 永久修改resolv.conf文件配置</h1>

1. 打开 powershell 输入  
$ wsl ~ <br/>
等待进入 <br/>
$ sudo nano /etc/resolv.conf<br/>
这里就会看到ip 又又被重置了 我这里是改好了的 如果没有处理的话<br/>
nameserver 192.168.120.1 <br/>
wsl开启的时候，随机生成一个<br/>

这里我去改文件，然后不再随机生成IP 我上网看了很多 像是resolvconf都试过了 ， 每次改了的文件，不能够永久替换<br/>

成品 ：<br/>
![image](https://user-images.githubusercontent.com/49250073/229793865-15be613c-aff9-427f-b744-4212bbf3e936.png)<br/>

### 记得 `IPv4 default gateway` and `IPv4 DNS Servers` 以下选一个方法来记得ip 等下会用<br/>
```
method 1 ：
打开Network connection 
选择你连接着的 Wifi or Ethernet 
右键 status
点击 details
记得 `IPv4 default gateway` and `IPv4 DNS Servers`
method 2 ：
在PowerShell run ipconfig /all
记得 `IPv4 default gateway` and `IPv4 DNS Servers`

```
---
2. 然后在powershell 写 
```
wsl ~  //进入Ubuntu
sudo nano /etc/wsl.conf
//打开后的编辑器 写入
```

> [network] <br/>
> generateResolvConf = false <br/>
> control+s Save control+x quit 保存退出
<br/>

---

3. 回去目录 ， 这里要删除文件 重新创建(在Ubuntu写入)
```
cd ~ 回去目录
sudo rm /etc/resolv.conf //移除文件
cd /etc //进入etc
sudo touch resolv.conf //创建回resolv.conf
sudo nano /etc/resolv.conf //改写里面的文件 可以看下图知道IP来源 在powershell 写 ipconfig /all
nameserver 192.168.0.1 
nameserver 8.8.8.8
nameserver 8.8.4.4 //不一定要三个nameserver， 选择一个default ipv4 一个ipv4 DNS就可以了

然后 control+s control+x 保存退出
```
---

4. 把上面记得/写入的 IPv4 default gateway & IPv4 DNS Servers 放进来 <br/>
由于我的是google dns所以我是放了8.8.8.8 / 8.8.4.4 <br/>
上网search 如何改成google dns如果你要的话， 不改就用自己的<br/><br/>
![image](https://user-images.githubusercontent.com/49250073/229799862-800cb133-3d70-4f94-a701-d8757cd8ee02.png)
<br/>

| 号码 |  解释           
|-----:|---------------
|     1|   这里我是用Eternet就是有线连接网络，如果是Wifi就找Wifi            
|     2|   这里是default gateway            
|     3|   这里是我的ipv4 dns 8.8.8.8 和 8.8.4.4            
---

5. 然后restart 你的wsl <br/>
打开一个新的powershell 
```
wsl --shutdown
然后 wsl ~
ping google.com 看有一大推的ping test出来
sudo nano /etc/resolv.conf 看文件有没有 自己又自动生成
如果是 :
nameserver 192.168.0.1 
nameserver 8.8.8.8
nameserver 8.8.4.4 
```
# 恭喜你 成功了 !

---
---

#第二个问题
##:wsl2 github unable to access的问题 ： 这里建议使用SSH 而不是HTTPS来clone project or push project 

以下操作是通过Ubuntu来生成ssh key<br/> 
然后绑定去github account

1. 先检查有没有ssh key
```
cd ~/.ssh
ls
```

如果有 <br/><br/>
![image](https://user-images.githubusercontent.com/49250073/229813034-167f73b6-dcd6-49ee-b418-2b9c3536b795.png)

---

2. 如果没有 <br/>
```
ssh-keygen -t rsa -C "ccr@gmail.com"  //Enter your github email
//输入后, 一直Enter,Enter, Enter...
```

---

3. 打印ssh key
```
cd ~/.ssh
cat id_rsa.pub
```
Copy ssh key <br/>
<br/><br/>
![image](https://user-images.githubusercontent.com/49250073/229814827-e8dbfb78-db74-4431-aa4f-8fcb1496cd29.png)

---

4. setting up ssh on github<br/>

A <br/>
![image](https://user-images.githubusercontent.com/49250073/229815609-10f96d8e-f903-4502-a5a5-9c500406c2ee.png) <br/>
B <br/>
![image](https://user-images.githubusercontent.com/49250073/229815750-8e3aa875-639b-4bad-b98a-1e5c299a2b7e.png)<br/>
C <br/>
![image](https://user-images.githubusercontent.com/49250073/229815863-29808d8c-8fc0-4352-b85d-b288e4afaf6a.png)<br/>
D <br/>
![image](https://user-images.githubusercontent.com/49250073/229815988-e6c57386-c7b1-41d3-89c2-1136564a5308.png)<br/>

5. 验证account是不是成功
```
ssh -T git@github.com
```

---

6. 验证成功信息
![image](https://user-images.githubusercontent.com/49250073/229816394-9f1ac0ef-7db7-47b7-9ee7-778cc9e4ac6a.png)





