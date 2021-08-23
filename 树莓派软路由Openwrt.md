[TOC]



# 树莓派软路由Openwrt教程

## 固件

https://github.com/SuLingGG/OpenWrt-Rpi

选factory.img的固件

并烧录

## 步骤

* ipconfig /all查看主路由器dns地址（本人为192.168.1.1）

* 连Openwrt的Wifi（无需密码）

* 电脑浏览器输入192.168.1.1(用户名:root,密码:password)

* “网络”->“接口”，编辑LAN；树莓派静态IP选192.168.1.254![img](https://img-blog.csdnimg.cn/20200222154001888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE1MzYwMzE=,size_16,color_FFFFFF,t_70)

* “保存&应用”，然后重启路由器。

* 将树莓派与主路由器用网线进行连接，在浏览器中输入 192.168.1.254

  

修改无线网：

* 网络-无线-修改-无线安全-加密

  

shh 登录

账号root密码password

## 注意事项

* 关闭树莓派的时候切忌热拔插，有概率损害文件系统。

如有关闭需要，最好ssh登录以后halt -f

* 请严格遵守流程，不然会出现玄学问题？

# 安装V2ray

V2ray是啥，懂得都懂，不懂的请自行百度，简而言之就是科学上网工具。

## 步骤

* https://github.com/kuoruan/luci-app-v2ray

  进Release Page                         ![QQ图片20210819115805.png](https://i.loli.net/2021/08/19/LseQYlM647RumhJ.png)

  ~~下载两个ipk备用~~       ![QQ图片20210819115810.png](https://i.loli.net/2021/08/19/97VAlu5O3Yyzbxa.png)
  请下载这个版本的两个ipk，不然会出玄学问题。
  ![QQ图片20210819135655.png](https://i.loli.net/2021/08/19/rMyphRuDZX2UqsL.png)

* 使用WinSCP工具https://winscp.net/eng/download.php （教程请另行搜索）将两个ipk传到/tmp目录下

* ssh登录 cd/tmp 到tmp目录下 ls检查一下ipk是否已经传输完成

* 在/tmp下

  opkg install luci-app-v2ray_*.ipk

  其中*是缩略，根据ipk版本不同改变，直接复制那两个ipk的名字即可

* ```
  opkg update
  opkg upgrade luci-app-v2ray
  opkg upgrade luci-i18n-v2ray-zh-cn
  ```

* 浏览器进192.168.1.254（你之前设置的树莓派静态IP）

* 服务-PassWall

* 接下来就自由发挥吧

  ![QQ图片20210819115814.png](https://i.loli.net/2021/08/19/TY8FtsG59wAyaXI.png)
  
  # 安装UU加速器插件

UU加速器是啥，懂得都懂。

官方教程：

https://router.uu.163.com/app/html/online/baike_share.html?baike_id=5f963c9304c215e129ca40e8?from_share_platform=QQ_FRIENDS

## 步骤

* 安装 kmod-tun

  ```shell
  opkg update
  opkg install kmod-tun
  ```

* 下载安装脚本

wget http://uu.gdl.netease.com/uuplugin-script/202012111056/install.sh -O install.sh

/bin/sh install.sh openwrt $(uname -m)

opkg update 

* 手机上安装uu主机加速

* 手机连软路由WIFI，如图修改手机网关（你软路由的网关）IP就用192.168.1.x即可（前面和路由器网关一致） （每个手机具体操作不同）![在这里插入图片描述](https://img-blog.csdnimg.cn/644b4598fd6141d8bab1ce8df223b62d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70#pic_center)


* 游戏主机连软路由WIFI，设置网关（每个游戏主机不一样，操作类似于手机，不再赘述）（我以PS5来举例）

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/57b6b3f74e5940fbaeaf32ebaec948de.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70#pic_center)


* 然后手机开UU主机加速，选路由器插件

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/fa36a1e408e54835a2920a1a19e59a8d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70#pic_center)


* 这样就成功了！![在这里插入图片描述](https://img-blog.csdnimg.cn/507415f8bb53422dbf16f365c9eb243c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70#pic_center)

# 数据同步syncthing

俗话说的好，一个好汉三个桩，一个篱笆三个帮。有些重要数据存放在单一的硬盘中，如果硬盘出了什么catastrophe的话，就不继续往下说了。

今天我们来部署的是syncthing，一个几乎能在所有平台上使用的**同步备份软件**。

## Openwrt上部署

进openwrt网关（192.168.1.254），网络储存-储存同步-启用（默认的8384口无需修改）

ssh到openwrt，登陆后键入syncthing就启动了。

打开我们同一局域网下的电脑（或手机或不管什么设备）在浏览器中192.168.1.254:8384就进入syncthing的网页管理端了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c9b54db58f74a17a20e65ac0e5d3e35.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70#pic_center)


使用方法也很简单，切换成你的语言，创建设置用户与密码（为了安全），添加远程设备，添加需要共享的文件夹。

只需要双方互为远程设备，且一方接受另一方的共享文件夹即可完成共享。

## Windows部署

https://github.com/canton7/SyncTrayzor/releases直接下载软件运行，设置方法同上。

## Android部署

谷歌商店直接搜Syncthing，设置方法同上。

## Linux云服务器部署

https://apt.syncthing.net/ 官方教程，别忘了防火墙开放端口8384，21025，22000

![请添加图片描述](https://img-blog.csdnimg.cn/cce7c790ab0c450fbbba30d39c6731f9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70)


第一次启动后会生成/root/.config/syncthing/ 用于保存配置文件以及证书、密钥(包括https证书、密钥)

vim /root/.config/syncthing/config.xml

把127.0.0.1:8384修改为0.0.0.0:8384

才能在浏览器中访问IP:8384。

# 内网穿透ZeroTier

zerotier这么好用的内网穿透软件相信很多人都接触过，网上教程也很多。

那我就来整个有点不一样的：zerotier部署在Openwrt上，局域网内设备都能自动连上zerotier。



* 还是Openwrt网关，ZeroTier-启用-输入你zerotier的ID，勾选‘自动允许客户端 NAT’-保存&应用

* 浏览器进zerotier-center，同意我们的路由器加入，等待zerotier给路由器自动分配的IP

* 然后到Advanced那栏![请添加图片描述](https://img-blog.csdnimg.cn/270b45deb07a40ce90e7e6bce6d16957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70)


  右边填zerotier给路由器自动分配的IP，左边填192.168.1.0/24，然后submit就大功告成了
