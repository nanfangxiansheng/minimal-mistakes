---
title: "Linux系统学习笔记（二）"
read_time: True
tags:
  - read time
---


# 鸟哥Linux私房菜笔记（二）

写于2025年1月22日，主要是鸟哥Linux私房菜的第二部分的第七章，第八章。还有第三部分的第九章和第十章。

## 第七章 Linux磁盘与文件系统管理

### 文件系统

Linux中的磁盘文件系统最早是ext2,管理文件并在其和物理存储间建立桥梁的是文件系统，在win98之间,windows用的是FAT文件系统，在win2000后是NTFS,而在linux中正统是ext2（Linux second extended file system）,win操作系统无法支持ext2.ext2中与文件有关的有三个部分，分别是：

1.超级区块，记录此文件系统的整体信息。

2.Inode:记录文件的属性，一个文件占用一个inode.

3.数据区块：记录文件的内容，当一个文件足够大的时候，会占用多个数据区块。

值得注意的是，ext2是索引式文件系统，而U盘，则不同了，U盘是FAT文件系统，就没有办法把文件一次性全部读取。每个文件系统都有其独立的inode,区块，超级区块等信息，这个文件系统要能够链接到目录树，才能被使用，将文件系统和目录树结合的操作称为挂载。挂载点一定是目录，该目录为进入该文件系统的入口。

![image-center1]({{ site.url }}{{ site.baseurl }}/assets/images/linux7.png){: .align-center}


现在Linux新发布的版本中，**一般是把XFS当作文件系统。**

### 文件系统的简单操作

调用df可以列出文件系统的整体磁盘使用量，调用du可以查看文件系统的磁盘使用量。1k-blocks指的是该空间可以被划分为多少个1kB，因为是wsl，所以还会打印出C盘和D盘的情况，确实如此，分别使用了60%和4%.

![image-center2]({{ site.url }}{{ site.baseurl }}/assets/images/linux8.png){: .align-center}

调用du结果会打印出大量的内容。

在Linux下面的链接文件有两种，一种是类似win的快捷方式功能的文件，可以快速的链接到目标文件，另外一种则是通过文件系统的inode链接来产生新文件名，而不是产生新文件，这种被成为**硬链接hard link.**一般来说，采用硬链接设置链接文件的时候，磁盘的空间与inode的数目都不会变化。

### 磁盘的分区，格式化，检验和挂载

当在系统中想要新增一块磁盘后，具体的操作步骤如下：

1.对磁盘进行划分，建立可用的硬盘分区。

2.对该硬盘分区进行格式化，以建立系统可用的文件系统

3.检验

4.在Linux系统上，需要设置挂载点，并将其挂载上来。

用lsblk可以列出系统上的所有的磁盘列表，用blkid可以列出设备的UUID参数，用parted可以列出磁盘的分区表类型。实践结果如下：

![image-center3]({{ site.url }}{{ site.baseurl }}/assets/images/linux9.png){: .align-center}

可知，lsblk可以查看磁盘列表，是常用的命令。

在后面，鸟哥还讲了一系列在linux系统上操作挂载，新增磁盘的方法，过于繁杂，实际上，一般装双系统的时候，这些都是提前规划好的。

此外，还讲了swap区的意义，swap区在物理内存不足的情况下才会被利用到。在安装linux系统时候会提前准备好，这里想来我装linux双系统已经装了三四次了，给我的教训就是一定在初始安装的时候要给linux留足硬盘空间，最起码根目录下有100个GB吧，否则到后面还是要重装，特别麻烦。

## 第八章文件与文件系统的压缩

在linux环境中，压缩命令和压缩包的格式特别多，在鸟哥linux中整理如下：

.tar 经过tar程序打包的文件，并没有压缩过。

.tar.gz tar程序打包的文件，并且经过了gzip的压缩。

.zip经过zip压缩过的文件

.gz经过gzip压缩过的文件。

通常的操作顺序是先用tar打包再用gzip压缩，gzip是GNU计划推出的压缩软件。同时目前出现的还有bzip2.具体示例如下：

![image-center4]({{ site.url }}{{ site.baseurl }}/assets/images/linux10.png){: .align-center}

在Linux系统中的打包工具是tar,tar可以将多个目录或文件打包成为一个大文件，同时还接受gzip,bzip2的支持，目前在win下的winRAR也支持.tar.gz文件名的解压缩。

tar所支持的操作简直繁多不胜数，可以用tar --help直接查看，常用的有tar -cf archive.tar foo bar .

![image-center5]({{ site.url }}{{ site.baseurl }}/assets/images/linux11.png){: .align-center}


实际上在有用户界面的情况下，压缩和解压缩十分的便捷，这让我想起了在功率半导体仿真实验上解压仿真示例的故事。

## 第九章VIM程序编辑器

vim编辑器的最大特点在于其具有程序编辑的能力，而且极其的轻量化。基本上vi分为三个模式，如下所示：

1.一般命令模式，command mode,一般以Vi打开后就直接进入该模式了。

2.编辑模式insert mode,按下i后才会进入编辑模式，要退回命令行模式，要按下ESC按钮

3.命令行模式command line mode.

具体的vim功能键十分复杂，详情参照鸟哥Linux私房菜-基础学习篇的第295页，这里不再赘述。

这里要注意的是，新建txt文本是vim test1.txt.进入编辑模式需要按下i键，随后要退出到命令行模式要**输入：**。这是很关键的，不退出到命令行模式在只读模式是没法保存的，这样就是相当于白编辑了。具体操作如下所示：

![image-center6]({{ site.url }}{{ site.baseurl }}/assets/images/linux12.png){: .align-center}

按下wq是保存并退出，这是最常用的。

vim真正好用在代码的编辑上面，假如你装的linux系统只有一个命令行界面，那就是没法用vscode的，只能使用vim编辑器了。

![image-center7]({{ site.url }}{{ site.baseurl }}/assets/images/linux13.png){: .align-center}

可见在vim中可以对不同的代码进行高亮现实，这对于单纯的命令行界面已经是革新的发明了，但是在图形界面当然不如vscode好用。

## 第十章认识和学习BASH

鸟哥Linux私房菜是目前我见过的讲Linux最好的书了，其讲的是相当的详细，还包含了大量的计组和操作系统的知识。鸟哥在书中说到：要是不学bash，那干脆别学linux系统了。

### 认识BASH这个shell

【天呐，上古程序员太厉害了】 https://www.bilibili.com/video/BV13Sc9eAEkB/?share_source=copy_web&vd_source=41c9fe6a45a4abff6dff06bd0a5ae744

这里多提一嘴，Intel公司在1975年成功开发的第一台天牛星8080个人计算机是真的靠拨动按钮来编写程序，或许在下学期的计组实验中，我会在FPGA上开发这样一台超微型的计算机，一般来说，有显示器和用户界面的出现和成功的应用要到1980年代后才有，2000年windows XP的出现几乎是划时代的图形用户界面操作系统，开创了win系统的时代直到现在。我所用过的最老的系统当属win6，win6的出现大概是2006年左右的事情，但是很可惜的是，我在我家的第一台win6电脑（还是二手的）上面所干过的事情无非是玩小游戏，这和家庭环境有关。顺带记录一下各大科技公司极其主营业务，Intel(CPU设计),AMD(CPU设计),Inivida(GPU设计)，Mcirosoft(操作系统开发),APPLE（用户设备开发）,IBM(现在主营专利出售，1990年代的科技巨头)，Google(用户软件开发)。

用户要直接和硬件接触显然是不可能的，因此一个APi就是BASH，用户先和Shell交互（Shell即壳程序）,Shell再和内核(Kernel)交互，kernel是Linux的根本，其直接负责和硬件接触。Linux系统上面的Shell是BASH，要注意这并不是一个英文单词，而是一个缩写，全称是Bourne again shell，win上面的Shell是Power shell.因此可知，在很大程度上，win上的shell 命令无法和linux的shell命令共用。另外补充一下windows上面的power shell几乎是目前最强大的shell工具，其理解起来非常简单，命令十分简单。

Bash shell的功能如下：

1.历史命令，可以查询历史命令

2.命令与文件补全功能，tab键的好处。

3.任务管理，前台后台控制

4.程序化脚本shell scripts.

### Shell的变量功能

变量是shell环境中的一个好帮手，重要的功能的存储和调用，变量的使用是通过：echo,而变量的设置是通过name=。举例如下：

![image-center8]({{ site.url }}{{ site.baseurl }}/assets/images/linux14.png){: .align-center}


环境变量可以实现很多功能，包括根目录的变换，提示字符的显示等等。查看默认的环境变量有env(environment variable)和export，如下所示：

![image-center9]({{ site.url }}{{ site.baseurl }}/assets/images/linux15.png){: .align-center}

打印输出较多，其中HOME是根目录，SHELL是告诉目前所用的哪个SHELL程序。实际上变量既可以通过命令行输入，也可以通过a=input("请输入一个变量的方式来输入")。例子如下：

![image-center10]({{ site.url }}{{ site.baseurl }}/assets/images/linux16.png){: .align-center}

### 命令别名与历史别名

Linux的 BASH shell允许我们去给命令设置别名，这在命令变得很长的时候尤其的管用。例如alias rm='rm -i'实测如下：

![image-center11]({{ site.url }}{{ site.baseurl }}/assets/images/linux17.png){: .align-center}

可见，这样设定后，以后删除文件的时候就不太会误删了，因为会有个选项。

在BASH中还提供查询历史命令的服务，具体操作方式如下：history 4,列出最近的四条命令。

![image-center12]({{ site.url }}{{ site.baseurl }}/assets/images/linux18.png){: .align-center}

此外还可以用history c来删除所有的内存中的命令。

![image-center13]({{ site.url }}{{ site.baseurl }}/assets/images/linux19.png){: .align-center}

### BASH中的重定向

在linux中的重定向特指抓取BASH命令行输出的信息并将其直接保存到别的文件中去。这在调试的时候会很有帮助，比如可以直接把一堆报错喂给AI（不一定是gpt,现在新出的deepseek在一些方面的表现还要强于gpt）.要注意的是，在linux中输出分为两种：标准输出和标准错误输出，而两种信息被重定向的方法也不一样。

具体示例如下：

在Linux中，可以使用重定向符号来将标准输出（stdout）和标准错误输出（stderr）重定向到文件或其他地方。以下是一些常见的例子：下面内容均来自于deepseek AI产生，实测比较好用。

#### 1. 将标准输出重定向到文件

bash

复制

```
command > output.txt
```

这会将 `command` 的标准输出重定向到 `output.txt` 文件中。如果文件不存在，则会创建它；如果文件已存在，则会覆盖它。

#### 2. 将标准输出追加到文件

bash

复制

```
command >> output.txt
```

这会将 `command` 的标准输出追加到 `output.txt` 文件的末尾，而不是覆盖它。

#### 3. 将标准错误输出重定向到文件

bash

复制

```
command 2> error.txt
```

这会将 `command` 的标准错误输出重定向到 `error.txt` 文件中。

#### 4. 将标准输出和标准错误输出重定向到同一个文件

bash

复制

```
command > output.txt 2>&1
```

这会将 `command` 的标准输出和标准错误输出都重定向到 `output.txt` 文件中。`2>&1` 表示将标准错误输出（文件描述符2）重定向到标准输出（文件描述符1）所在的位置。

#### 5. 将标准输出和标准错误输出分别重定向到不同的文件

bash

复制

```
command > output.txt 2> error.txt
```

这会将 `command` 的标准输出重定向到 `output.txt`，而标准错误输出重定向到 `error.txt`。

#### 6. 将标准输出和标准错误输出都追加到同一个文件

bash

复制

```
command >> output.txt 2>&1
```

这会将 `command` 的标准输出和标准错误输出都追加到 `output.txt` 文件的末尾。

#### 7. 将标准输出重定向到文件，标准错误输出丢弃

bash

复制

```
command > output.txt 2> /dev/null
```

这会将 `command` 的标准输出重定向到 `output.txt`，而标准错误输出会被丢弃（`/dev/null` 是一个特殊的设备文件，写入它的内容会被丢弃）。

#### 8. 将标准输出和标准错误输出都丢弃

bash

复制

```
command > /dev/null 2>&1
```

这会将 `command` 的标准输出和标准错误输出都丢弃。

![image-center14]({{ site.url }}{{ site.baseurl }}/assets/images/linux20.png){: .align-center}

在该示例中，正确的输出信息被非覆盖保存在new.txt中，报错输出被非覆盖保存在error.txt，后面打开error.txt如下所示：

![image-center15]({{ site.url }}{{ site.baseurl }}/assets/images/linux21.png){: .align-center}

此外值得补充的是，在linux中的输入命令间也有执行的判断逻辑，想要一次执行多条指令用";"例子如下：

![image-center16]({{ site.url }}{{ site.baseurl }}/assets/images/linux22.png){: .align-center}

### 管道命令(PIPE)

在Linux系统中，管道（Pipe）是一种强大的工具，用于将一个命令的输出作为另一个命令的输入。管道通过竖线符号 `|` 实现，可以将多个命令串联起来，形成复杂的数据处理流程。管道中可以匹配的加工命令有很多。

#### 1.**结合 `grep` 过滤输出**

`grep` 是常用的文本过滤工具，可以与管道结合使用：

bash

复制

```
command | grep pattern
```

- **示例**：查看进程列表，并过滤出包含 `nginx` 的进程：

  bash

  复制

  ```
  ps aux | grep nginx
  ```

------

#### 2. **结合 `awk` 处理文本**

`awk` 是一种强大的文本处理工具，可以提取和操作特定字段：

bash

复制

```
command | awk '{print $1}'
```

- **示例**：列出当前目录下的文件，并只显示文件名（第一列）：

  bash

  复制

  ```
  ls -l | awk '{print $9}'
  ```

------

#### 3. **结合 `sort` 排序**

`sort` 可以对输出进行排序：

bash

复制

```
command | sort
```

- **示例**：列出当前目录下的文件，并按文件名排序：

  bash

  复制

  ```
  ls | sort
  ```

------

#### 4. **结合 `uniq` 去重**

`uniq` 可以去除重复的行，通常与 `sort` 结合使用：

bash

复制

```
command | sort | uniq
```

- **示例**：统计日志文件中出现的唯一IP地址：

  bash

  复制

  ```
  cat access.log | awk '{print $1}' | sort | uniq
  ```

------

#### 5. **结合 `wc` 统计行数、单词数或字符数**

`wc` 可以统计输入的行数、单词数或字符数：

bash

复制

```
command | wc -l
```

- **示例**：统计当前目录下的文件数量：

  bash

  复制

  ```
  ls | wc -l
  ```

------

#### 6. **结合 `xargs` 将输出作为参数**

`xargs` 可以将管道传递的输出作为另一个命令的参数：

bash

复制

```
command | xargs command2
```

- **示例**：删除当前目录下所有 `.tmp` 文件：

  bash

  复制

  ```
  find . -name "*.tmp" | xargs rm
  ```

------

#### 7. **结合 `tee` 同时输出到屏幕和文件**

`tee` 可以将输出同时显示在屏幕上并写入文件：

bash

复制

```
command | tee file.txt
```

- **示例**：将 `ls` 的输出保存到 `files.txt` 并显示在屏幕上：

  bash

  复制

  ```
  ls | tee files.txt
  ```

![image-center17]({{ site.url }}{{ site.baseurl }}/assets/images/linux23.png){: .align-center}

可知,linux的BASH出名的原因是花活比较多，但是值得注意的是，像上面的这些花活在win的power shell中也有。像ls | tee new.txt这样的操作是很有用的，可以同时保存和打印输出命令，这对于一些安装的debug很有用。但是要注意的是，管道命令仅仅会接收标准输出，而不接收标准错误输出。
未来还要学习第11章和第12章，但这得等到美赛后了。

### 



