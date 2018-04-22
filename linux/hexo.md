## 什么是 Hexo？

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://markdownpad.com/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装Hexo
安装hexo其实很简单，只需要短短的几分钟就可以完成，只是因为hexo是基于Node.js我们需要提前安装Node.js环境。Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以在非浏览器环境下，解释运行 JS 代码。
### 安装git
*   Windows：下载并安装 [git](https://github.com/git-for-windows/git/releases/download/v2.16.2.windows.1/Git-2.16.2-64-bit.exe).
*   Linux (Ubuntu, Debian)：`sudo apt-get install git-core`
*   Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`

### 安装Node.js
Node.js下载地址：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

>#### Windows 用户
>对于windows用户来说，建议使用安装程序进行安装。安装时，请勾选Add to PATH选项。
另外，您也可以使用Git Bash，这是git for windows自带的一组程序，提供了Linux风格的shell，在该环境下，您可以直接用上面提到的命令来安装Node.js。打开它的方法很简单，在任意位置单击右键，选择“Git Bash Here”即可。由于Hexo的很多操作都涉及到命令行，您可以考虑始终使用Git Bash来进行操作。

这里我们介绍一些linux环境下的安装方法。我这里使用的是已经安装好的[node.js源码包](https://nodejs.org/dist/v8.11.1/node-v8.11.1-linux-x64.tar.xz)，直接解压缩就可以用。因此我这里将其解压缩到/usr/local/下然后添加环境变量，使node.js的命令在linux环境下可以直接使用。具体代码如下：

```Shell
tar xf node-v8.11.1-linux-x64.tar.xz -C /usr/local/
echo 'export PATH=$PATH:/usr/local/node-v8.11.1-linux-x64/bin/' > /etc/profile.d/node.sh
export PATH=$PATH:/usr/local/node-v8.11.1-linux-x64/bin/
```
安装完node.js后我们就可以使用npm命令安装hexo了。
```Shell
npm install hexo-cli -g
```
到这里我们就已经将hexo安装完成了。就是这么简单。

## 生成博客静态页面

安装 Hexo 完成后，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```Shell
hexo init <folder>
cd <folder>
npm install
```
到这里一个简单的个人博客网站就出来了，只是只有一个主页和一个测试页面而已。如果想要快速生成所有的页面只需要将你编写的markdown格式的博客文件放到<folder>/source/_posts/目录下，然后生成通过hexo快速生成所有博客页面的静态文件了。hexo生成静态文件的命令如下：
```Shell
hexo generate     #（hexo g 也可以） 
```
到这一步后你个人博客网站的静态页面就生成好了存放在<folder>/public目录下。如果我们需要用nginx发布自己的网站就只需要将<folder>/public目录下的所有文件放到你nginx的目录下即可。

### 本地预览
启动hexo本地预览服务的命令：
```Shell
hexo server     #（hexo s 也可以） 
```
执行以上启动本地预览服务命令后，在浏览器输入[http://localhost:4000]()进行效果预览。
>**注意：**hexo server启动的默认端口是4000端口，如果在hexo是建立在云服务器（没有图形化界面）上的是不能使用[http://localhost:4000]()这时候我们要进行预览就得使用云服务器的的公网IP和端口号进行预览了，预览的时候还需要注意防火墙是否开放了4000端口的访问。虚拟机用户也需要注意防火墙是否打开了该端口。

### 变更主题
使用Hexo更换主题还算方便，先使用克隆命令从互联网上下载hexo主题文件到本地<folder>/themes/下，然后更改一下博客的配置文件_config.yml里面的主题名称就好了。

###### 使用git下载主题：
这里以下载hexo官网提供的[AlphaDust](https://github.com/klugjo/hexo-theme-alpha-dust)为例，将它放在博客目录的themes目录下的alphadust（如果该目录不存在会自动新建）目录下。hexo提供的[主题](https://hexo.bootcss.com/themes/)。

```
git clone https://github.com/klugjo/hexo-theme-alpha-dust <folder>/themes/alphadust
```

###### 启用主题：
启用主题只需要修改配置文件_config.yml中的theme后的值即可，这里修改为：
```
theme: alphadust
```
>**注意：**
>1.在修改配置文件时“theme:”和值之间是有空格的，如果没有空格在更新主题将不会生效。
>2.值为你博客目录下themes目录下的文件名，如果在themes目录下没有对应的目录，那么更新主题同样也不会生效。

###### 更新主题：
在主题更新之前，一定要备份好主题目录下的_config.yml文件，尤其是到后面修改了很多配置之后。在themes/alphadust目录下执行:
```
git pull origin master
```
>**说明：**上面这里的命令是用于连接到了github的用户更新网站主题的命令，对于本地用户不用执行上面的命令在修改配置文件后```hexo s```启动本地预览服务器就可以预览到效果。当然也可以生成静态文件，然后放到nginx服务器去预览。

更多关于_config.yml配置参数的使用见：[https://hexo.bootcss.com/docs/configuration.html](https://hexo.bootcss.com/docs/configuration.html)