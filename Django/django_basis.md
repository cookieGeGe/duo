## Django

Django是一个开放源代码的Web应用框架，由Python写成。是一个基于MVC构造的框架。但是在Django中采用了MVT的软件设计模式，即模型Model，视图View和模板Template。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。并于2005年7月在BSD许可证下发布。

### MVC框架

MVC框架，它强制性的使应用程序输入、处理和输出分开。使用MVC应用程序被分成三个核心部件：模型、视图、控制器。它们各自处理自己的任务。

MVC设计模式核心：

解耦，让不同代码块之间降低耦合，增强代码的可扩展和可移植性，实现向后兼容。

- M(Model):数据存取层，主要封装对数据库层的访问，实现对数据库中的数据进行增、删、改、查操作。

- V(View):表现层，用于封装结果，生成向页面展示的html页面，或返回数据给用户。

- C(Controller):业务逻辑层，用于接收用户请求，处理业务逻辑，与Model和View交互，返回结果。

MVC流程分析：

![MVC流程分析图](https://upload-images.jianshu.io/upload_images/10930505-01eae26ac15ec44b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### MVT模式

Django采用的是MVT模型，MVT和MVC有一点不同之处，但是本质上其实都是差不多的，只是换了一个说法：

- M(Model):与MVC中的M功能相同，负责和数据库交互，进行数据处理。

- V(View):与MVC中的C功能相同，接收请求，进行业务处理，返回应答。

- T(Template):与MVC中的V功能相同，负责封装构造要返回的html。

在Django中还有一个分发器，所有来自用户的请求都先交由分发器处理，分发器处理后将用户请求交由对应的View进行业务处理。

### 安装Django

在实际开发中我们需要处理多个不同的项目，各个项目用的Django版本或者其他的库可能不尽相同，所以我们需要在虚拟环境中开发。

##### 创建虚拟环境

##### windows环境

1.安装virtualenv：

```pip install virtualenv```

2.创建虚拟环境：

```virtualenv --no-site-packages ./env1```

>说明：--no-site-packages指定不创建除pip等几个关键的库之外的其他任意库；-p在多版本解释器的情况下，用于指定python解释器的路径，如果只有一个python解释器可以不用指定；最后还需要指定创建env的路径。

如图：

![创建虚拟环境](https://upload-images.jianshu.io/upload_images/10930505-ac1ed0a428000e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.进入虚拟环境安装Django

```
cd env1/Scripts/
activate   #  启动虚拟环境
pip install django==1.11   # 在虚拟环境中安装指定版本的Django
```

如图：

![安装django](https://upload-images.jianshu.io/upload_images/10930505-4dbe4345b21608fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>说明：安装django的时候可以指定版本，如果不指定版本则默认安装最新版本

4.退出虚拟环境使用deactivate

##### Centos7安装

在Centos7中创建虚拟环境和在windows环境下创建虚拟环境的步骤其实差不多，只是centos中已经预装了python 2.7版本，我们自己安装了python3.x在创建虚拟环境的时候需要指定版本。

```
pip3 install virtualenv  # 安装虚拟环境

virtualenv --no-site-packages -p /usr/local/python3/bin/python /mnt/virtual/env/  #/mnt/virtual/env下创建虚拟环境

cd /mnt/virtual/env/bin/

source activate  # 启动虚拟环境

pip install django==1.11  # 在虚拟环境中安装django 1.11

deactivate  # 退出虚拟环境

```

如图：

![安装虚拟环境](https://upload-images.jianshu.io/upload_images/10930505-5d8bc06ccd0e8e2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![创建虚拟环境](https://upload-images.jianshu.io/upload_images/10930505-6308f0042cd168f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![安装Django](https://upload-images.jianshu.io/upload_images/10930505-ea1b3c39883a8f2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 创建第一个Django项目

在虚拟环境中创建第一个Django项目：

```
django-admin startproject project_name
```

windows和linux环境下创建Django的示例：

![windows下创建Django](https://upload-images.jianshu.io/upload_images/10930505-3bb194d612e65008.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![linux下创建Django](https://upload-images.jianshu.io/upload_images/10930505-9f2dc5005d8009f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 预览创建的helloworld项目

要预览Django项目也必须在创建的虚拟环境下执行下面的命令才可以：

```
cd helloworld
python manage.py runserver
```

如图：

![windows启动项目](https://upload-images.jianshu.io/upload_images/10930505-1764a4449774d229.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![windows预览图](https://upload-images.jianshu.io/upload_images/10930505-a6219ce47dee3b04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![linux启动图](https://upload-images.jianshu.io/upload_images/10930505-1c89f81ae31df5d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![linux预览图](https://upload-images.jianshu.io/upload_images/10930505-267ed63b060d7321.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>注意：如果在启动时没有看到如上图成功的页面，原因可能有：一、配置为没有设置所有主键可以访问；二、防火墙没有打开8000端口。在启动helloworld项目时，runserver后面可以指定参数[ip:端口号]，如果没有指定则默认是127.0.0.1:8000

