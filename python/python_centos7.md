## CentOS 7安装Python 3.6.4

###一、解决依赖关系

在 CentOS 7 中安装 Python 3.6.4之前，请确保系统中已经有了所有必要的依赖包否则会报错：
```Shell
yum -y groupinstall development
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
```
>**说明：**以上两个包很重要，如果没有提前安装就会安装失败。查看本机上是否安装使用命令：“yum list | grep 包名”和“yum grouplist”进行查看。

###二、安装前准备：

安装包的下载：
第一种方法：
在[python官网](https://www.python.org/)下载源码格式的安装包，然后通过winscp或者xftp上传到你的服务器。
![Python 3.6.4源码下载页面](https://upload-images.jianshu.io/upload_images/10930505-1a7d1b6ac0d75b08.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第二种方法：
使用wget命令联网在线下载：
```Shell
wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz
```

###三、安装Python 3.6.4

找到通过上传或者下载到服务器的Python-3.6.4.tgz的安装包。并执行解压缩：
```Shell
tar xf Python-3.6.4.tgz
cd Python-3.6.4
```
解压缩后进入到源码目录，进行编译安装(通常用户安装的应用程序是放在/usr/local目录下的，这里我们将python3.6.4安装在/usr/local/python3目录下)：
```Shell
./configure --prefix=/usr/local/python3
make && make install
```
>*注意：*
>1.如果/usr/local目录下没有python3这个文件夹，在进行编译安装的时候会自动创建，所以可以不必提前创建文件夹。
>2.在执行‘./configure --prefix=/usr/local/python3’这条命令时，如果出现一下错误：“configure: error: no acceptable C compiler found in $PATH”，提示错误信息“没有找到合适的C编译器”，这是由于没有安装gcc导致的，所以执行‘yum install -y gcc’安装gcc后重新执行上面两条命令即可。

###四、添加到PATH环境变量

linux环境下和windows环境中安装Python 3.x版本有一点不一样，在windows下安装时可以通过勾选添加到PATH环境变量自动添加，但是在linux环境下需要我们手动添加，添加方法如下：
```Shell
cd /etc/profile.d
echo 'export PATH=$PATH:/usr/local/python3/bin/' > python3.sh
```
>**说明：**
>1.通常在添加环境变量的时候是单独为该程序在/etc/profile.d目录创建一个文件去修改环境变量，这样是方便以后查找和取消添加的环境变量。
>2.添加到PATH环境变量的路径为Python安装路径下的bin目录。

执行上面的命令添加环境变量后并不是立即生效的，需要退出登录后重新登录才会生效（这个方法是永久有效的重启服务器后也能生效）。如果想要立即生效就执行命令‘export PATH=$PATH:/usr/local/python3/bin/’。


###五、验证安装是否成功

验证是否安装成功其实很简单，只需要在终端中输入python3即可。
![成功安装后执行命令的提示](https://upload-images.jianshu.io/upload_images/10930505-1b20709c12db1976.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果你是按照我上面的教程一步一步的做，并且也没有报其他错误的话，相信你执行python3这个命令，一定可以看到上面的页面，如果看到上面面的页面就表示已经成功了。如果没有看到也不要气馁，仔细看看是不是哪里报错了你没有发现，然后再来看看教程，你一定会成功的。