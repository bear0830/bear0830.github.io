VPS搭建LAMP安装WordPress建站 (搬瓦工VPS亲测 Mac OS环境操作)


#### #1 为什么要用WordPress建站

[WordPress](https://wordpress.org/)是一种使用PHP语言和MySQL数据库开发的个人博客系统，其稳定可靠，易于使用，且是免费开源的。而最让我看重的，是它支持一大波优秀的插件和模板，比如SEO优化、静态缓存和数据备份等。

具体可参看百度文库相关介绍：[http://baike.baidu.com/item/WordPress](http://baike.baidu.com/item/WordPress)

#### #2 注册域名

考虑到性价比（免费隐私保护）和支付便利（支持支付宝），博主目前在用以下两个域名注册商，在这也推荐给大家。

*阿里云（万网）：[https://wanwang.aliyun.com/domain/](https://wanwang.aliyun.com/domain/)*

*NameSilo：[https://www.namesilo.com/](https://www.namesilo.com/)*

#### 2020年12月31日前，使用NameSilo优惠码 the1dollar 可减免一美元，首年只需 $4.99（续费 $5.99/年）。

#### #3 如何购买搬瓦工VPS

搬瓦工 KVM-512MB [直达链接](https://bwh88.net/cart.php?aff=16180)

搬瓦工可以使用支付宝（Alipay）非常方便。

打开[搬瓦工（BandwagonHost）官网](https://bwh88.net/cart.php?aff=16180)，选择10G-VPS这款。

![image](https://upload-images.jianshu.io/upload_images/6402511-4fdaaf843c302d54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后选择**年付19.99美元**（下拉选择），推荐美国西海岸的洛杉矶机房。QNET和MCOM都可以，博主测试的速度都差不多。

![image](https://upload-images.jianshu.io/upload_images/6402511-9ea6d617b66b581f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

记得使用搬瓦工优惠码，还能再省一点点银子。建议试试这个优惠码：**BWH1ZBPVK**

![image](https://upload-images.jianshu.io/upload_images/6402511-5fa3ef068bd7143e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来填写注册信息，之后选择付款方式。推荐支付宝（Alipay）

![image](https://upload-images.jianshu.io/upload_images/6402511-6d792f4107f396d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

稍等片刻，完成后会有邮件提示。登陆后台（Client Area），打开My Services菜单。

![image](https://upload-images.jianshu.io/upload_images/6402511-e7f0e74acad87965.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在就能看见新建的VPS了！我们需要登陆KiwiVM控制面板进行VPS管理。

![image](https://upload-images.jianshu.io/upload_images/6402511-750aee9ec64618b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此，就可以看到比较详细的VPS信息了。主要包括IP地址、SSH端口、内存和空间使用量等。记下IP和SSH端口，在下文中使用Putty登陆SSH时会用到。

![image](https://upload-images.jianshu.io/upload_images/6402511-2e4ed9d2d9d3fff4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来安装系统。这里选择Centos-6-x86（32位）。重装之后会显示新的root密码和SSH端口，记得保存下来，后面登陆SSH时会用到。

![image](https://upload-images.jianshu.io/upload_images/6402511-32f3e0b3a1808e7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面就可以通过SSH管理VPS了

#### #4 SSH连接VPS

SSH（Secure Shell）即安全外壳协议，是目前较可靠、专为远程登录会话和其他网络服务提供安全性的协议。我们需要一种SSH工具来连接VPS，Windows用户可以选择SecureCRT或PuTTY客户端，Mac OS用户则可以直接用终端进行连接。

博主采用了Mac OS进行SSH连接，具体方式如下：

1 在Mac OS上打开终端Terminal

2 ssh root@搬瓦工VPS的IP地址 -p 端口号

具体例子如

ssh root@12.34.56.78 -p 12345

由于搬瓦工VPS没有采用默认的SSH 22端口号，所以你需要登录到搬瓦工的终端上进行一下查询

查询方法：

1 登录搬瓦工控制台 [https://kiwivm.64clouds.com/main.php](https://kiwivm.64clouds.com/main.php)

2 进入 Root Shell Basic 菜单

3 输入 netstat -anp| grep ssh    即查询你自己的搬瓦工所使用的SSH的端口

#### #5 搭建LAMP环境

LAMP指的是Linux（操作系统）、Apache（HTTP服务器），MySQL（数据库软件） 和PHP（有时也是指Perl或Python）的第一个字母，主要用来建立web应用平台。

博主使用的是LNMP一键安装包，具体可参看这里：[https://lnmp.org/install.html](https://lnmp.org/install.html)

# screen -S lnmp

回车，创建screen会话。

# wget -c ftp://soft.vpser.net/lnmp/lnmp1.3-full.tar.gz && tar zxf lnmp1.3-full.tar.gz && cd lnmp1.3-full && ./install.sh **lamp**

回车，进入搭建LAMP环境前的必要配置。

以下安装过程不再赘述，主要设置详见下图。

这里设置的数据库ROOT密码务必记牢，下面添加域名时会用到！！

![image](https://upload-images.jianshu.io/upload_images/6402511-3dd374580cfa8412.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-b9d104fa49d12fd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-0b8e532ff3740f09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当出现上图中的绿字 “Press any key to install…or Press Ctrl+c to cancel” 后，按回车键确认开始安装。

安装大约持续半个小时左右。安装成功后的界面如下图所示：

![image](https://upload-images.jianshu.io/upload_images/6402511-0bfe758fd07e408c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此，LAMP环境已经在VPS上搭建完成。输入VPS的IP访问，会出现以下界面。

![image](https://upload-images.jianshu.io/upload_images/6402511-c8241d5ba0ee5ebd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

提示：为了安全，建议将phpmyadmin目录重命名为不容易猜到的目录！（比如hereispma）

在安装WordPress之前，**建议安装PHP缓存加速类扩展**，对降低VPS压力和提高WordPress速度大有裨益。

推荐安装两个：OPcache和Memcached。

首先，需要进入LNMP解压目录lnmp1.3-full：

# cd /root/lnmp1.3-full

回车，接下来安装Opcache：

#**.**/addons.sh install opcache

回车，再回车。

当出现 “Opcache installed successfully, enjoy it!” 字样时，即表示安装成功。

接着安装Memcached：

#**.**/addons.sh install memcached

回车，选择2，回车，再回车。

当出现 “Memcached installed successfully, enjoy it!” 字样时，即表示安装成功。

此时，可以删除之前下载的lnmp1.3安装包，以节省空间。

# rm -rf /root/lnmp1.3-full.tar.gz

回车即可。

接下来就可以添加域名安装WordPress了。

#### #6 添加域名 / 虚拟主机

# lnmp vhost add

回车，提示输入域名：

# yourhost.com

回车，提示是否添加多个域名：

# y

回车，博主习惯绑定带www的域名：

# www.yourhost.com

回车。博主习惯不需要日志记录。

# n

回车后，输入站长邮箱。

继续回车，提示数据库名和数据库用户名是否保持一致。

# y

回车，输入root用户的数据库密码

当出现下图所示画面时候，说明添加域名已经成功。

![image](https://upload-images.jianshu.io/upload_images/6402511-c0713f05c2867db8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### #7 安装WordPress程序

以下的步骤想必应该很熟悉，和带Cpanel或DirectAdmin面板安装WordPress过程比较类似。只不过，在面板上操作是可视化的，比较直观。而在这里是通过命令执行的，非可视。只要输入命令时细心点，一般是不会出问题的。

首先，进入添加的域名目录：

# cd /home/wwwroot/yourhost.com

回车。然后打开[WordPress中文站点](https://cn.wordpress.org/)，下载程序压缩包：

# wget https://cn.wordpress.org/wordpress-4.5.3-zh_CN.tar.gz

回车。等待下载完之后，解压压缩包：

# tar -zxvf wordpress-4.5.3-zh_CN.tar.gz

回车。

接下来，将解压出来的wordpress文件夹内全部文件移动到当前的域名目录下（别忘了后面的**.**）。

# mv wordpress/* **.**

回车。然后，可以选择删掉空文件夹wordpress。

# rm -rf wordpress

回车，搞定。

为避免因权限的问题导致安装出错，**比如wp-config.php无法创建、需要提供FTP用户密码以及主题和插件不能更新等**，建议赋予根目录文件的可写权限。

# chmod -R 755 /home/wwwroot

回车。

# chown -R www /home/wwwroot

回车。

提示：以后每添加一个域名，都要执行一次以上两步操作。

另外，LNMP安装包默认禁用了scandir函数，这会导致WordPress后台看不到安装的主题，以及当前主题总显示 “有新的翻译可用” 的提醒。所以，需要开启此函数。

# vi /usr/local/php/etc/php.ini

回车，然后查找scandir函数。

# ?scandir

回车，然后按delete键删除，接下来需要保存并退出vi命令。

#**:**wq

回车。然后重启一下LNMP：

# lnmp restart

回车。

最后，通过你的浏览器打开博客网址进行最后的安装吧！

通常为 http://yourhost.com/wordpress

至此，恭喜你已经完成了从域名购买，到搬瓦工VPS搭建，到WordPress建站的全部流程。

WordPress是一种使用PHP语言和MySQL数据库开发的个人博客系统，其稳定可靠，易于使用，且是免费开源的。而最让我看重的，是它支持一大波优秀的插件和模板，比如SEO优化、静态缓存和数据备份等。

具体可参看百度文库相关介绍：http://baike.baidu.com/item/WordPress

#2 注册域名

考虑到性价比（免费隐私保护）和支付便利（支持支付宝），博主目前在用以下两个域名注册商，在这也推荐给大家。

阿里云（万网）：https://wanwang.aliyun.com/domain/

NameSilo：https://www.namesilo.com/
2020年12月31日前，使用NameSilo优惠码 the1dollar 可减免一美元，首年只需 $4.99（续费 $5.99/年）。


#3 如何购买搬瓦工VPS

搬瓦工 KVM-512MB 直达链接

搬瓦工可以使用支付宝（Alipay）非常方便。

打开搬瓦工（BandwagonHost）官网，选择10G-VPS这款。

然后选择年付19.99美元（下拉选择），推荐美国西海岸的洛杉矶机房。QNET和MCOM都可以，博主测试的速度都差不多。

记得使用搬瓦工优惠码，还能再省一点点银子。建议试试这个优惠码：BWH1ZBPVK

接下来填写注册信息，之后选择付款方式。推荐支付宝（Alipay）

稍等片刻，完成后会有邮件提示。登陆后台（Client Area），打开My Services菜单。

现在就能看见新建的VPS了！我们需要登陆KiwiVM控制面板进行VPS管理。

至此，就可以看到比较详细的VPS信息了。主要包括IP地址、SSH端口、内存和空间使用量等。记下IP和SSH端口，在下文中使用Putty登陆SSH时会用到。

接下来安装系统。这里选择Centos-6-x86（32位）。重装之后会显示新的root密码和SSH端口，记得保存下来，后面登陆SSH时会用到。

下面就可以通过SSH管理VPS了
#4 SSH连接VPS

SSH（Secure Shell）即安全外壳协议，是目前较可靠、专为远程登录会话和其他网络服务提供安全性的协议。我们需要一种SSH工具来连接VPS，Windows用户可以选择SecureCRT或PuTTY客户端，Mac OS用户则可以直接用终端进行连接。

博主采用了Mac OS进行SSH连接，具体方式如下：

1 在Mac OS上打开终端Terminal

2 ssh root@搬瓦工VPS的IP地址 -p 端口号

具体例子如

ssh root@12.34.56.78 -p 12345

由于搬瓦工VPS没有采用默认的SSH 22端口号，所以你需要登录到搬瓦工的终端上进行一下查询

查询方法：

1 登录搬瓦工控制台 https://kiwivm.64clouds.com/main.php

2 进入 Root Shell Basic 菜单

3 输入 netstat -anp| grep ssh    即查询你自己的搬瓦工所使用的SSH的端口


#5 搭建LAMP环境

LAMP指的是Linux（操作系统）、Apache（HTTP服务器），MySQL（数据库软件） 和PHP（有时也是指Perl或Python）的第一个字母，主要用来建立web应用平台。

博主使用的是LNMP一键安装包，具体可参看这里：https://lnmp.org/install.html

# screen -S lnmp

回车，创建screen会话。

# wget -c ftp://soft.vpser.net/lnmp/lnmp1.3-full.tar.gz && tar zxf lnmp1.3-full.tar.gz && cd lnmp1.3-full && ./install.sh lamp

回车，进入搭建LAMP环境前的必要配置。

以下安装过程不再赘述，主要设置详见下图。

这里设置的数据库ROOT密码务必记牢，下面添加域名时会用到！！



当出现上图中的绿字 “Press any key to install…or Press Ctrl+c to cancel” 后，按回车键确认开始安装。

安装大约持续半个小时左右。安装成功后的界面如下图所示：

至此，LAMP环境已经在VPS上搭建完成。输入VPS的IP访问，会出现以下界面。

提示：为了安全，建议将phpmyadmin目录重命名为不容易猜到的目录！（比如hereispma）

在安装WordPress之前，建议安装PHP缓存加速类扩展，对降低VPS压力和提高WordPress速度大有裨益。

推荐安装两个：OPcache和Memcached。

首先，需要进入LNMP解压目录lnmp1.3-full：

# cd /root/lnmp1.3-full

回车，接下来安装Opcache：

#./addons.sh install opcache

回车，再回车。

当出现 “Opcache installed successfully, enjoy it!” 字样时，即表示安装成功。

接着安装Memcached：

#./addons.sh install memcached

回车，选择2，回车，再回车。

当出现 “Memcached installed successfully, enjoy it!” 字样时，即表示安装成功。

此时，可以删除之前下载的lnmp1.3安装包，以节省空间。

# rm -rf /root/lnmp1.3-full.tar.gz

回车即可。

接下来就可以添加域名安装WordPress了。


#6 添加域名 / 虚拟主机

# lnmp vhost add

回车，提示输入域名：

# yourhost.com

回车，提示是否添加多个域名：

# y

回车，博主习惯绑定带www的域名：

# www.yourhost.com

回车。博主习惯不需要日志记录。

# n

回车后，输入站长邮箱。

继续回车，提示数据库名和数据库用户名是否保持一致。

# y

回车，输入root用户的数据库密码

当出现下图所示画面时候，说明添加域名已经成功。

#7 安装WordPress程序

以下的步骤想必应该很熟悉，和带Cpanel或DirectAdmin面板安装WordPress过程比较类似。只不过，在面板上操作是可视化的，比较直观。而在这里是通过命令执行的，非可视。只要输入命令时细心点，一般是不会出问题的。

首先，进入添加的域名目录：

# cd /home/wwwroot/yourhost.com

回车。然后打开WordPress中文站点，下载程序压缩包：

# wget https://cn.wordpress.org/wordpress-4.5.3-zh_CN.tar.gz

回车。等待下载完之后，解压压缩包：

# tar -zxvf wordpress-4.5.3-zh_CN.tar.gz

回车。

接下来，将解压出来的wordpress文件夹内全部文件移动到当前的域名目录下（别忘了后面的.）。

# mv wordpress/* .

回车。然后，可以选择删掉空文件夹wordpress。

# rm -rf wordpress

回车，搞定。

为避免因权限的问题导致安装出错，比如wp-config.php无法创建、需要提供FTP用户密码以及主题和插件不能更新等，建议赋予根目录文件的可写权限。

# chmod -R 755 /home/wwwroot

回车。

# chown -R www /home/wwwroot

回车。

提示：以后每添加一个域名，都要执行一次以上两步操作。

另外，LNMP安装包默认禁用了scandir函数，这会导致WordPress后台看不到安装的主题，以及当前主题总显示 “有新的翻译可用” 的提醒。所以，需要开启此函数。

# vi /usr/local/php/etc/php.ini

回车，然后查找scandir函数。

# ?scandir

回车，然后按delete键删除，接下来需要保存并退出vi命令。

#:wq

回车。然后重启一下LNMP：

# lnmp restart

回车。

最后，通过你的浏览器打开博客网址进行最后的安装吧！

通常为 http://yourhost.com/wordpress

至此，恭喜你已经完成了从域名购买，到搬瓦工VPS搭建，到WordPress建站的全部流程。

