---
title: virtualbox从u盘启动进入Ubuntu
tags:
  - virtualbox
  - ubuntu
categories:
  - virtualbox
abbrlink: 3314239952
date: 2018-04-05 21:09:23
---

1.将U盘接到电脑的USB接口上。键盘上按WIN+R打开运行窗口，输入diskmgmt.msc点击确定打开磁盘管理。
<!-- more -->
![diskmgmt.msc](virtualbox从u盘启动进入Ubuntu\01f9cc8a.png)
![Disk Management](virtualbox从u盘启动进入Ubuntu\798f477b.png)

2.然后我们右击VirtualBox程序，选择以管理员身份打开。这个现在只要打开就可以，暂不进行操作。
![Run as administrator](virtualbox从u盘启动进入Ubuntu\889384e6.png)

3.以管理员身份打开CMD窗口。（这里要注意，一定要以管理员身份打开CDM，否则执行命令时要权限不足。）

4.先查看一下VirtualBox的安装目录位置，等下需要进入到这个目录。右击VirtualBox图标，点击属性。在起始位置中可以看安装的盘符及具体的安装目录。把这个复制起来。
![VirtualBox](virtualbox从u盘启动进入Ubuntu\1ae20c79.png)

5.现在我们在CMD窗口中，要进入到VirtualBox的安装目录位置。
![从CMD进入VirtualBox](virtualbox从u盘启动进入Ubuntu\744df697.png)

6.接着输入
> VBoxManage internalcommands createrawvmdk -filename F:\usb.vmdk -rawdisk \\.\PhysicalDrive1

PhysicalDrive1里面的1是第一步里面的u盘位置。

7.然后按下回车键执行，就创建成功。我们进入到刚才的这个保存位置去看看有没有usb.vmdk文件，有的话就表示创建成功。

8.在virtualbox里新建
![创建虚拟机](virtualbox从u盘启动进入Ubuntu\79c730a1.png)
![从u盘启动](virtualbox从u盘启动进入Ubuntu\26c48efa.png)