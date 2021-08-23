# OpenWrt云服务器-1

本人使用的Raspberry4B作为OpenWrt软路由，24小时开机，性能强大。转念一想，不把他当作云服务器耍耍真的可惜了，所以俺们来整一整它。

## OpenWrt 安装pip,pyOpenSSL,setuptools

### 步骤

* ```javascript
  opkg update
  ```

* python查看一下python的版本，若未安装python则需opkg install python

* #### 安装setuptools

  * https://pypi.org/project/setuptools/下载符合版本的setuptools

  * winscp到\tmp目录，tar -zxvf  *.tar.gz解压，进入解压后的目录。

  * ```bash
    # 批量修改当前文件夹里面的文件的修改日期
    find . -type f -exec touch {} + 
    ```

  * ```javascript
    python setup.py install
    ```

* #### 安装pyOpenSSL

  * https://pypi.org/project/pyOpenSSL/#files
  * 相同操作

* #### 安装pip

  * https://pypi.org/project/pip/#files
  * 相同操作

## OpenWrt定时邮件

### 步骤

* **pip install PyEmail** （用来import smtplib）
* 写一个demo试一试，python 通过SMTP发邮件具体教程网上很多*
 ```python
import smtplib

from email.mime.text import MIMEText

smtpHost = 'smtp.qq.com'

mail_user = "XXX@qq.com"

sender = 'XXX@qq.com'

password = "XXX"# 授权码

receiver = '984549371@qq.com'

content = 'hello qq.com'

msg = MIMEText(content)

msg['Subject'] = 'email-subject'

msg['From'] = sender

msg['To'] = receiver

## smtp ssl port 465

smtpServer = smtplib.SMTP_SSL(smtpHost, 465)  # SMTP_SSL

smtpServer.login(mail_user, password)

smtpServer.sendmail(sender, receiver, msg.as_string())

smtpServer.quit()
  ```
发现可以正常收信

* ```s
  # 我烧的这版OpenWrt是在这个目录
  vim /etc/crontabs/root
  ```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c10d32ddba2f41378518d32543044d81.png)

  crontab的规范：

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-7voTrd82-1629438023454)(C:\Users\sekiro\AppData\Roaming\Typora\typora-user-images\image-20210820133142679.png)\]](https://img-blog.csdnimg.cn/d0a3ab39285d468c891a9a05c93d7cd0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTExMTYxMw==,size_16,color_FFFFFF,t_70)


* 这样的话我们只要winscp我们需要定时执行的程序，然后再在crontab里面编写好运行周期即可。（vim的使用网上也有很多教程）
