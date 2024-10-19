
# 如何修改Hosts文件

## 1.什么是 DNS 污染

>有时你会发现，某些网站在国内网站无法访问，一般有三种可能：1.这个网站关闭了；2.被墙了；3.被DNS 污染了。像在国内被屏蔽的中小型网站网站就属于第 3 种，通过修改电脑和手机Hosts文件多数可以解决这个问题。但是像Google、Youtube、Facebook、Twitter等这些大网站和手机上的APP是直接ip都被墙了，这种办法就行不通了，只能找梯子翻墙访问。

[![dns-wuran](https://www.safewebcn.com/img/dns-wuran-min.png)](#1%E4%BB%80%E4%B9%88%E6%98%AF-dns-%E6%B1%A1%E6%9F%93)

hosts文件可以视为一个系统本地上启用的DNS服务器，我们通过编辑它能把域名定向到正确的ip，可以解锁在国内受到DNS污染的网站服务。hosts是一个系统文件，在Windows系统下位于`C:\windows\system32\drivers\etc`，这个文件没有后缀，你可以用文字处理软件去编辑它，要注意的是它是一个系统保护的文件，所以需要用管理员权限才可以编辑。

## 2.DNS污染咋解决

修改电脑和手机`Hosts`文件，可以绕过网络运营商的DNS，为域名指定正确的 IP 地址，达到访问被屏蔽网站的目的。Hosts文件的位置如下（安卓需要 root，iPhone 需要越狱，电脑Windows/Mac/Linux可以直接修改）

### 2.1 Hosts文件位置

Windows系统: C:\Windows\System32\drivers\etc

>技术小白建议在电脑上进行操作，更为方便。

直接把文件位置复制到文件夹地址栏，按`Enter`键就看到了。

[![hosts](https://www.safewebcn.com/img/hosts-min.png)](#21-hosts文件位置)

Android/iPhone/Mac/Linux: /etc/hosts

### 2.2 如何修改Hosts

**方法一：** 这种最简单

国内Windows/Mac用户可以直接下载hosts文件**解锁本文中所有VPN官网**（手机用户建议在电脑上打开本页面操作）：

1.下载hosts文件： <a rel="nofollow noopener" href="http://linkv.org/download/hosts" target="_blank"> 下载地址</a>(右键打开)

2.Windows用户将下载的hosts文件移动到文件夹 `C:\Windows\System32\drivers\etc` （Mac的文件夹是 `/etc/hosts`），建议备份系统自带的原始hosts文件。

3.完成之后清空浏览器缓存、刷新浏览器。

完成之后，就可以正常访问官网和使用服务了。

**打开VPN官网测试：**

>如果还是打不开，请关闭浏览器再重新进入此页面点击下方链接进入官网。

- [**获取StrongVPN最新优惠**](https://linkv.org/strongcn/)([StrongVPN中国使用提示](https://vpncn.github.io/#2-strongvpn--%E6%80%A7%E4%BB%B7%E6%AF%94%E6%9C%80%E4%BD%B3))

- [**获取NordVPN官网优惠**](https://linkv.org/nord/)

- [点击获取PureVPN官网优惠](https://linkv.org/pure/)


**方法二：** 手动修改

以Windows系统为例，打开文件夹C:\Windows\System32\drivers\etc，把 hosts文件**移动到桌面**，打开方式选择记事本。

[![hosts](https://www.safewebcn.com/img/hosts-open-min.png)](#22-如何修改hosts)

在末尾处添加ip地址和域名（之间加一个空格），**保存文件并重新移动回** C:\Windows\System32\drivers\etc **覆盖掉之前的Host文件**，最后打开浏览器设置，清空浏览器缓存，即可访问。

[![](https://www.safewebcn.com/img/hosts-edit-min.png)](#22-如何修改hosts)

## 网站域名与对应 IP

直接复制下面的ip解析行，插入到Host文件最末端，保存之后，再移动到原来的文件夹覆盖。然后**刷新浏览器或打开浏览器设置清空缓存**再点击下方链接即可进入官网。

>如果还是打不开，请关闭浏览器再重新进入此页面点击下方链接进入官网。



### NordVPN

插入这3行：

`54.171.39.37 go.nordvpn.net`

`54.171.39.37 nordvpn.net`

`52.44.235.100 get.affiliatescn.net`

添加完成，保存然后重启浏览器，[**获取NordVPN官网优惠**](http://linkv.org/nord)。

### StrongVPN

插入5行：

`69.16.145.224 strongvpn.com`

`69.16.145.224 intranet.strongvpn.com`

`69.16.145.213 links.strongvpn.com`

`192.200.155.38 affiliate.strongvpn.com`

`104.16.53.111 support.strongvpn.com`

添加完成，保存然后重启或使用另一浏览器，点击[**获取StrongVPN最新优惠**](https://linkv.org/strongcn/)

### PureVPN

插入这5行：

`104.16.37.8 purevpn.com`

`104.18.4.178 billing.purevpn.com`

`45.33.2.97 affiliates.purevpn.com`

`104.16.113.39 www.purevpn.com`

`104.18.0.55 my.purevpn.com`

添加完成后保存，然后刷新浏览器，[点击获取PureVPN官网优惠](https://linkv.org/pure/)，应该可以打开官网了。

### IVacy

插入这2行：

`104.18.200.99 www.ivacy.com`

`104.18.85.5 billing.ivacy.com`

添加完成后保存，然后刷新浏览器，[点击获取IVacy官网优惠](https://linkv.org/ivacy/)
