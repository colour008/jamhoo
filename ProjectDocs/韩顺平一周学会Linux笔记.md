---
title: 韩顺平一周学会Linux笔记📒
date: 2025-11-19 20:00:00
updated: 2025-11-22 20:00:00
tag: [Linux,Centos]
categories: [知识库,Linux]
cover:
description: 韩顺平一周学会Linux笔记 📒
swiper_index: 1
sticky: 1
---
---

# 一、Centos 安装

## 1. 基础分区

基础分区必须包含`boot`**（引导分区，一般分1G大小）**、`swap`**（交换分区，一般分与内存相同大小）**和`root`**（根分区，大小不限）**三个分区，一般格式化为`ext4`。

![image-20251119202409330](https://bu.dusays.com/2025/11/19/691db6f07988d.png)

## 2. Vmware网络连接模式

- **桥接模式**：虚拟机可以和宿主机及其他主机直接通讯，但容易造成宿主机局域网内IP冲突；
- **NAT模式**：即网络地址转换模式，虚拟机通过宿主机虚拟网卡与宿主机实现通讯，不会造成宿主机局域网内的IP冲突，但是其他主机无法访问虚拟机；
- **仅主机模式**：独立的系统，是一种比NAT模式更加封闭的的网络连接模式，它将创建完全包含在主机中的专用网络。

> 参考文章：[Vmware—桥接、NAT以及仅主机模式的详细介绍和区别](https://zhuanlan.zhihu.com/p/532535216)

## 3. 虚拟机克隆

如果已经安装了一台linux操作系统，还想再更多的，没必要再重新安装，只需要克隆就可以，方法如下：

* **方式1**：直接拷贝一份安装好的虚拟机文件，然后通过vmware文件→打开→选择vmx文件即可。也可跨主机实现克隆。顺便填一下，迁移的话可以直接剪切，删除的话直接删除即可。

* **方式2**：使用vmware的克隆操作，鼠标选择要克隆的系统，然后点击右键选择管理→克隆，然后按需点击下一步即可。

> 注意：在进行克隆时，需要先关闭要克隆的linux系统

## 4. 虚拟机快照

如果你在使用虚拟机系统的时候(比如linux)，你想回到原先的某一个状态，也就是说你担心可能有些误操作造成系统异常，需要回到原先某个正常运行的状态，vmware也提供了这样的功能，就叫快照管理。示例如下：

* 安装好系统以后，先做一个快照A；
* 进入到系统，创建一个文件夹，再保存一个快照B；
* 回到系统刚刚安装好的状态，即快照A；
* 试试看，是否还能再次回到快照B。

![image-20251120202916604](https://bu.dusays.com/2025/11/20/691f099cf1475.png)

## 5.  安装vmtools

步骤如下：

* 进入centos点击vm菜单的→install vmware tools；

* centos会出现一个vm的光驱型安装包，打开后里面有类似xx.tar.gz的压缩文件；

* 将xx.tar.gz拷贝到`/opt`目录；

* 通过终端使用`cd /opt`命令进入`opt`目录，然后使用`tar -zxvf  xx.tar.gz`命令解压,得到一个安装文件夹；

* 通过`ls`查看解压的产生的目录，然后`cd`到解压目录；

* 通过`ls`查看目录文件，然后使用`./vmware-install.pl`安装，全部按<kbd>回车键</kbd>使用默认设置即可，就可以安装成功。

> 注意:安装vmtools<span style="font-size:1.1em; color:#CC0000;">需要有gcc </span>

## 6. 添加共享文件夹

vmtools安装后，可以在windows下更好的管理vm虚拟机，也可以设置windows和centos的共享文件夹。添加共享文件夹后，在Linux系统通过打开主文件夹→其他位置→计算机→`mnt`→`hgfs`→共享文件夹名，进入到共享文件夹内。

> 实际生产环境，通过远程方式实现文件的上传和下载。

设置好共享文件夹后，重启系统进入`/mnt/hgfs`，发现没有权限查看，找不到共享文件夹，无法显示，可以在终端执行如下命令解决：

```bash
sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
```

如果提示挂载过了`fuse: mountpoint is not empty`，再加一个 `-o nonempty`参数即可，具体如下：

```bash
sudo vmhgfs-fuse .host:/ /mnt/hgfs -o nonempty -o allow_other
```

再进入`/mnt/hgfs`即可看到共享的文件，亲测可行。但是，可能下次开机又要重复上述操作，如果每次重启之后想让系统自动挂载，就编辑`/etc/fstab`，具体如下：
```bash
vim /etc/fstab

# 打开/etc/fstab可以看到，对应6个的字段，系统启动时会自动将字段中的文件挂载到指定位置

<file system> <mount point> <type> <options> <dump> <pass>

# 在/etc/fstab最后添加一行，正好对应6个字段：

.host:/ /mnt/hgfs fuse.vmhgfs-fuse allow_other 0 0
```

这样重启时就会自动挂载好了。如果继续想在`Ubuntu/CentOS`桌面直接打开文件夹，需要建立文件夹的快捷方式。方法如下:

**方法1**：左手按住<kbd> Ctrl </kbd> + <kbd>Shift</kbd>，右手控制鼠标，将文件夹拖拽至桌面。

**方法2**：
2.1 Ubuntu：进入任意终端，输入如下命令（可用`pwd`命令查询当前路径）：

```bash
$ sudo ln -sT [文件夹路径] [桌面文件夹路径]

#例如1
$ sudo ln -sT /home/user_name/文件夹/ /home/user_name/桌面/文件夹

#例如2
$ sudo ln -sT /home/user_name/文件夹 /home/user_name/桌面/文件夹
```


2.2 CentOS 7：按照2.1方法执行时如果报错`XXX is not in the sudoers file.  This incident will be reported.`，先执行`su`，再执行2.1（不输`sudo`）可解决。

# 二、目录结构

> <span style="font-weight:bold; font-size:1.2em; color:#FF0000;"> 记住一句经典的话：在Linux世界里，一切皆文件！</span>

![img](https://bu.dusays.com/2025/11/22/6921996870077.jpg)

`/bin`：常用目录，包括（`/usr/bin`、`/usr/local/bin`），是Binary的缩写，存放最经常使用的命令。

`/sbin`：包括（`/usr/sbin`、`/usr/local/sbin`），s就是Super User的意思，存放系统管理员使用的系统管理程序。

`/home`：常用目录，存放普通用户的主目录。

`/root`：常用目录，为系统管理员目录。

`/lib`：系统开机所需最基本的动态连接共享库，类似于Windows里的DLL文件。

`/lost+found`：一般情况下是空的且隐藏的，当系统非法关机后，就存放一些文件。

`/etc`：常用目录，所有系统管理所需要的配置文件和子目录，比如MySQL数据库的my.conf文件。

`/usr`：常用重要目录，用户的很多应用程序和文件都放在该目录下，类似于Windows下的program files 文件夹。

`/boot`：常用重要目录，存放linux启动时的一些核心文件。

`/proc`：重要目录不能动，是一个虚拟目录，是系统内存的映射。

`/srv`：service的缩写，存放一些服务启动后需要提取的数据。

`/sys`：系统文件，是Linux2.6内核的的一个很大的变化。安装了2.6内核中新出现的一个文件系统。

`/tmp`：存放一些临时文件的目录。

`/dev`：device的缩写，类似于Windows的设备管理器，把所有的硬件以文件的形式存储。

`/media`：常用目录，Linux系统会自动识别一些设备，比如U盘、光驱等，当识别后，Linux会把识别的设备挂载到该目录下。

`/mnt`：常用目录，为了让用户临时挂载别的文件系统的，可以将外部的存储挂载到`/mnt/`上，然后进入该目录就可以查看里面的内容了，比如上面挂载的共享文件夹share目录。

`/opt`：给主机提供额外存放安装软件的目录，如安装Oracle数据库就可以放到该目录下，默认为空。

`/usr/local`：l另一个给主机提供额外存放安装软件的目录，一般是通过编译源码方式安装的程序。

`/var`：存放着不断扩充的东西，习惯将经常被修改的目录放在该目录下，包括各种日志文件。

`/selinux`：security-enhanced linux的缩写，是一种安全子系统（类似安全防护系统），它能控制程序只能访问特定文件，有三种工作模式，可以自行设置。

> 参考：[菜鸟教程网-Linux 系统目录结构](https://www.runoob.com/linux/linux-system-contents.html)





