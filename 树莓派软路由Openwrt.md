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

关闭树莓派的时候切忌热拔插，有概率损害文件系统。

如有关闭需要，最好ssh登录以后poweroff

# 安装V2ray

V2ray是啥，懂得都懂，不懂的请自行百度，简而言之就是科学上网工具。

## 步骤

* https://github.com/kuoruan/luci-app-v2ray

  进Release Page                         ![image-20210819114826842](C:\Users\sekiro\AppData\Roaming\Typora\typora-user-images\image-20210819114826842.png)

  下载两个ipk备用       ![image-20210819114925511](C:\Users\sekiro\AppData\Roaming\Typora\typora-user-images\image-20210819114925511.png)

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

  ![image-20210819115534901](C:\Users\sekiro\AppData\Roaming\Typora\typora-user-images\image-20210819115534901.png)