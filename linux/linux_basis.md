### linux系统简介

**Linux**是一种[自由和开放源代码](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E5%8F%8A%E5%BC%80%E6%94%BE%E6%BA%90%E4%BB%A3%E7%A0%81%E8%BD%AF%E4%BB%B6 "自由及开放源代码软件")的[类UNIX](https://zh.wikipedia.org/wiki/%E7%B1%BBUnix%E7%B3%BB%E7%BB%9F "类Unix系统")[操作系统](https://zh.wikipedia.org/wiki/%E4%BD%9C%E6%A5%AD%E7%B3%BB%E7%B5%B1 "操作系统").该操作系统的[内核](https://zh.wikipedia.org/wiki/%E5%86%85%E6%A0%B8 "内核")由[林纳斯·托瓦兹](https://zh.wikipedia.org/wiki/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9 "林纳斯·托瓦兹")在1991年10月5日首次发布。在加上用户空间的应用程序之后，成为Linux操作系统。Linux也是自由软件和[开放源代码软件](https://zh.wikipedia.org/wiki/%E5%BC%80%E6%94%BE%E6%BA%90%E4%BB%A3%E7%A0%81%E8%BD%AF%E4%BB%B6 "开放源代码软件")发展中最著名的例子。只要遵循[GNU通用公共许可证](https://zh.wikipedia.org/wiki/GNU%E9%80%9A%E7%94%A8%E5%85%AC%E5%85%B1%E8%AE%B8%E5%8F%AF%E8%AF%81 "GNU通用公共许可证")，任何个人和机构都可以自由地使用Linux的所有底层源代码，也可以自由地修改和再发布。大多数Linux系统还包括像提供[GUI](https://zh.wikipedia.org/wiki/GUI "GUI")的[X Window](https://zh.wikipedia.org/wiki/X_Window "X Window")之类的程序。除了一部分专家之外，大多数人都是直接使用[Linux发行版](https://zh.wikipedia.org/wiki/Linux%E7%99%BC%E8%A1%8C%E7%89%88 "Linux发行版")，而不是自己选择每一样组件或自行设置。---------引自维基百科[linux介绍](https://zh.wikipedia.org/wiki/Linux)

###### linux发行版：

截止今日linux已经有很多个发行版本，但是现在使用较多的做的比较好的发行版还是：Ubuntu、Debian、SUSE、Red Hat、Centos、Fedora。因为在Centos是红帽的社区版本基本和红帽一样且是完全免费的所以在国内大多数中小企业都选中Centos作为服务器的操作系统。

### linux目录结构

![linux系统目录结构](https://upload-images.jianshu.io/upload_images/10930505-4afc160789adaa66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- /：根目录，在目录树的最顶端，所有的目录都在根下。一般根目录下只存放目录，不要存放文件。
- /bin和/usr/bin:可执行二进制文件的目录，普通用户可以使用的命令就放在这个目录下。如常用的命令ls、tar、mv、cat等。
- /boot：放置linux系统启动时用到的一些文件。
- /dev：存放linux系统下的设备文件，访问该目录下某个文件，相当于访问某个设备，常用的是挂载光驱mount /dev/cdrom /mnt。
- /etc：系统配置文件存放的目录，在操作此目录下的文件时要慎重，错误操作可能会导致系统或者某项服务不能正常启动。重要的配置文件有/etc/inittab、/etc/fstab、/etc/init.d、/etc/sysconfig/*等，修改配置文件之前记得备份。
- /home：除root用户以外的其他用户的默认家目录都在改目录下，通常建议将/home目录单独分区，并设置较大的磁盘空间，方便用户存放数据。
- /lib和/usr/lib和/usr/local/lib：系统或程序执行时依赖的一些库文件的存放目录，一般来说不会对这些库文件进行操作。
- /lost+fount：系统异常产生错误时，会将一些遗失的片段放置于此目录下，通常这个目录会自动出现在装置目录下。如加载硬盘于/disk 中，此目录下就会自动产生目录/disk/lost+found。
- /mnt和/media：挂载点，硬盘、光盘、U盘等设备都是挂载在这两个目录下，光盘通常挂载在/mnt/目录下。
- /opt：给主机额外安装软件所摆放的目录。
- /proc：proc文件系统是一个伪文件系统，它只存在于内存当中（只有系统启动起来该目录下才会有文件），而不占用外存空间。它以文件系统的方式为访问系统内核数据的操作提供接口。用户和应用程序可以通过proc得到系统的信息，并可以改变内核的某些参数。由于系统的信息，如进程，是动态改变的，所以用户或应用程序读取proc文件时，proc文件系统是动态从系统内核读出所需信息并提交的。
- /root：系统管理员root的家目录。
- /sbin和/usr/sbin和/usr/local/sbin：放置系统管理员使用的可执行命令，如fdisk、shutdown、mount等。与/bin不同的是，这几个目录是给系统管理员root使用的命令，一般用户只能"查看"而不能设置和使用。
- /tmp：一般用户或正在执行的程序临时存放文件的目录,任何人都可以访问,重要数据不可放置在此目录下。
- /srv：服务启动之后需要访问的数据目录。
- /usr：应用程序存放目录，/usr/bin存放应用程序，/usr/share存放共享数据，/usr/lib存放不能直接运行的，却是许多程序运行所必需的一些函数库文件。/usr/local:存放软件升级包。/usr/share/doc:系统说明文件存放目录。/usr/share/man: 程序说明文件存放目录，使用 man ls时会查询/usr/share/man/man1/ls.1.gz的内容建议单独分区，设置较大的磁盘空间。
- /var：放置系统执行过程中经常变化的文件，如随时更改的日志文件/var/log，/var/log/message：所有的登录文件存放目。录，/var/spool/mail：邮件存放的目录，/var/run:程序或服务启动后，其PID存放在该目录下。建议单独分区，设置较大的磁盘空间。

## 总结<hr>

linux的重要哲学思想是一切皆文件。这是 Unix/Linux 的基本哲学之一。不仅普通的文件，目录、字符设备、块设备、 套接字等在 Unix/Linux 中都是以文件被对待；它们虽然类型不同，但是对其提供的却是同一套操作界面。只有正确的理解了linux操作系统的哲学思想才能很好玩转linux。
