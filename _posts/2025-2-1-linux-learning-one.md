---
title: "Linux系统学习笔记（一）"
read_time: false
tags:
  - read time
---

# 鸟哥私房菜linux学习笔记(一)

之所以学鸟哥linux私房菜这本书，是因为受到多人推荐，本书在业界也一直好评，买来一看，名副其实，从最基础的计算机构成开始讲起，适合爱好者和从业者学习或者深入研究。本书有若干章节，我挑选了部分来学习，重点学习文件管理和shell.

## 第一章linux的起源

unix作为古早的系统于1971年左右诞生，随后到了1984年，stallman发起了开源软件运动GNU计划，同年，Minix系统发行。到了1991年的时候芬兰人linus开发出来Linux系统，其实全名应当位Linux-GNU系统，Linux发行版如ubuntu,centos,debian等等是由内核（linux kernel）,自由软件，工具等构成的。

## 第二章主机规划与磁盘分区

### 磁盘分区

在磁盘分区主要有两种方法，分别是MBR和GPT，检查方式是：在win系统下->磁盘管理->属性->硬件->属性->写入。就可以看到是MBR还是GPT了，一般来说，普通电脑都是MBR的，是传统型，GPT是新出现的，支持的磁盘容量更大。
![image-center1]({{ site.url }}{{ site.baseurl }}/assets/images/linux1.png){: .align-center}

而所谓NFTS是win的文件管理系统，New Technology File System。

### 启动方式

按下开机键后，第一个运行的程序是BIOS,这和普通的嵌入式设备不同，嵌入式设备通电就跑主程序，对于复杂的计算机，先跑的是BIOS(basic input output system),然后是MBR(连接磁盘)，然后是boot loader(引导操作系统到内存中)，然后操作系统的内核开始运行。

现在除了BIOS外还有个UEFI(unified extensible firmware interface),UEFI的全程是UEFI bios是下一代的启动引导接口，目的是取代传统的BIOS,UEFI几乎就是一个低级的操作系统，不同厂家的笔记本的UEFI BIOS都是不太相同的，在开机后对于机械革命是狂按F10就可以进入到BIOS界面，在里面还需要去关掉Secure boot,secure boot在有些时候会阻止进入linux系统。

### linux安装时候磁盘分区的选择

linux系统最特殊的在于其目录树架构。

![image-center2]({{ site.url }}{{ site.baseurl }}/assets/images/linux2.png){: .align-center}

根目录是linux系统最重要的目录，因为所有的目录都是从根目录中衍生而来，在/boot放置和开机有关的档案，在/etc中是与系统设置有关的档案，在/home中是预设的系统家目录，/mnt是暂时挂载用的,/root是系统管理员的家目录，在/home中有常用的desktop文件夹。在/usr（全名是unix software resource并非user，从中可以看出linux带有unix的遗产）中放置了系统的大量软件资源，和win下的c:\program files类似.在/var中放置大量系统运行中产生的临时文件如缓存等等。

在wsl2这个win下的linux系统中ls一下，效果是这样的：装的还是22.04长支持的ubuntu.里面可以清晰的看到鼎鼎大名的GNU

![image-center3]({{ site.url }}{{ site.baseurl }}/assets/images/linux3.png){: .align-center}


具体可以参考这篇帖子：

[深入理解linux系统的目录结构（总结的非常详细）_linux目录结构-CSDN博客](https://blog.csdn.net/yup1212/article/details/82152106)

在linux中文件系统是EXT4(**Fourth Extended File System**),文件系统和根目录的关系是挂载，也就是进入该目录就可以读取相应的磁盘分区。

## 第五章linux的文件权限和目录配置

### linux文件特征

在Linux的每个文件中分别给了用户，用户组（在团队协作的时候很有用，比如某微电子专业的数字集成电路设计实验上就有两人协同在linux系统上用cadence设计数字芯片的实例），其他人三种权限。

在linux文件管理中，命令行输入ls -l后可以列出所有的文件属性，在其中的第一个字符串中的二，三，四三个字符分别代表了用户，用户组，其他人的权限。

![image-center4]({{ site.url }}{{ site.baseurl }}/assets/images/linux4.png){: .align-center}

1. r是读取目录中的内容（读取文件的实际内容）

2. w是修改目录中的内容（可以编辑该文件）

3. x是访问目录（该文件可以被系统执行权限）

   

要开放目录给其余人使用的话，应当至少给予r和x的权限，w权限不可提供。

### linux目录配置

linux目录配置的依据是Filesystem hierarchy standard标准，主要有三层目录，分别为/（根目录），/usr(与软件的安装和执行有关)，/var与系统运行过程有关。绝对路径和相对路径的出现是为了方便系统索引找到指定文件的，绝对路径是从根目录开始的，相对路径是从当前所处目录开始的。

在win系统中所谓系统环境变量就是绝对路径，一般软件间协作通信都需要绝对路径放置在环境变量中。

## 第六章linux文件与目录管理

### 目录的相关操作

前面提到了绝对目录和相对目录的区别，绝对目录是不会出错的，类似的来说，在python很多读取文件的操作的时候，总是会建议将相对路径换成绝对路径。常见的目录操作如下所示：

1.cd change directory切换目录

2.pwd print working directory 打印当前目录

3.mkdir make directory新建目录

4.rmdir remake directory删除目录

具体示例如下，在win->wsl2->ubuntu22.04LTS操作,想要安装wsl2可以在b站搜相关视频

![image-center5]({{ site.url }}{{ site.baseurl }}/assets/images/linux5.png){: .align-center}

输入echo $PATH后可以打印输出环境变量

![image-center6]({{ site.url }}{{ site.baseurl }}/assets/images/linux6.png){: .align-center}

在该图中的环境变量较多是因为是win下的wsl，会把win的环境变量也打印出来。

### 文件与目录管理

ls可以列出所有的文件，调用cp,rm,mv分别是复杂，删除，移动。在cp中还有许多参数，不同身份去做cp的结果也不一样，最高权限是sudo。
查看文件的内容可以用cat，cat可以从第一行开始显示文件内容。

### 