## Django 日志

日志在程序开发中是少不了的，通过日志我们可以分析到错误在什么地方，有什么异常。在生产环境下有很大的用途。那么在 django 中是怎么处理日志？ django 利用的就是 Python 提供的 logging 模块，但 django 中要用 logging 模块，还得有一定的配置规则，需要在 setting 中设置。

### logging 模块

logging 模块为应用程序提供了灵活的手段记录事件、错误、警告和调试信息。对这些信息可以进行收集、筛选、写入文件、发送给系统日志等操作，甚至还可以通过网络发送给远程计算机。

- logging 模块的组成：

	Loggers 向应用程序提供log接口，每个logger 是一个具名的容器，可以向它写入需要处理的消息。

	Handlers 决定如何处理logger 中的每条消息，如将log记录发送到指定的目的地（控制台输出，或写入文件或发向网络等等）

	Filters 提供一个分级策略控制，对从logger 传递给handler 的日志记录进行额外的控制。

	Formatters 指定输出的最终格式，格式化输出日志。

### 日志级别

logging模块的重点在于生成和处理日志消息。每条消息由一些文本和指示其严重性的相关级别组成。按日志严重性由上向下递减。

|:--:|:--:|
| 级别 | 描述 |
| CRITICAL | 关键错误/消息 |
| ERROR | 错误 |
| WARNING | 警告消息 |
| INFO | 通知消息 |
| DEBUG | 调试消息 |
| NOTSET | 无级别 |

### 记录器

记录器负责管理日志消息的默认行为，包括日志记录级别、输出目标位置、消息格式以及其它基本细节。

|:--:|:--:|
| 关键字参数 | 描述 |
| filename | 将日志消息附加到指定文件名的文件 |
| filemode | 指定用于打开文件模式 |
| format | 用于生成日志消息的格式字符串 |
| datefmt | 用于输出日期和时间的格式字符串 |
| level | 设置记录器的级别 |
| stream | 提供打开的文件，用于把日志消息发送到文件。 |

### format 日志消息格式

|:--:|:--:|
| %(name)s | 记录器的名称 |
| %(levelno)s | 数字形式的日志记录级别 |
| %(levelname)s | 日志记录级别的文本名称 |
| %(filename)s | 执行日志记录调用的源文件的文件名称 |
| %(pathname)s | 执行日志记录调用的源文件的路径名称 |
| %(funcName)s | 执行日志记录调用的函数名称 |
| %(module)s | 执行日志记录调用的模块名称 |
| %(lineno)s | 执行日志记录调用的行号 |
| %(created)s | 执行日志记录的时间 |
| %(asctime)s | 日期和时间 |
| %(msecs)s | 毫秒部分 |
| %(thread)d | 线程ID |
| %(threadName)s | 线程名称 |
| %(process)d | 进程ID |
| %(message)s | 记录的消息 |

### 内置处理器

logging模块提供了一些处理器，可以通过各种方式处理日志消息。使用addHandler()方法将这些处理器添加给Logger对象。另外还可以为每个处理器配置它自己的筛选和级别。

- handlers.DatagramHandler(host，port):发送日志消息给位于制定host和port上的UDP服务器。

- handlers.FileHandler(filename):将日志消息写入文件filename。

- handlers.HTTPHandler(host, url):使用HTTP的GET或POST方法将日志消息上传到一台HTTP 服务器。

- handlers.RotatingFileHandler(filename):将日志消息写入文件filename。如果文件的大小超出maxBytes制定的值，那么它将被备份为filename1。

通常用的是最后一种，方便日志的自动管理。

### 配置django日志

编辑项目目录下的settings.py文件：

#### 1.创建日志的存储目录：

```
# 指定存储日志的目录为项目目录下的log目录下
LOG_PATH = os.path.join(BASE_DIR, 'log')
# 判断log目录是否存在，如果不存在则创建该目录
if not os.path.isdir(LOG_PATH):
    os.mkdir(LOG_PATH)
```

#### 2.定义Logging的格式

```
LOGGING = {
	# 固定version为1
    'version': 1,
    # disable_existing_loggers表示是否禁用日志功能，默认为True。
    'disable_existing_loggers': False,

    # 定义日志的输出格式：
    'formatters': {
        # 此处可以定义多种不同的输出格式，并为其命名
        'verbose': {
            'format': '%(levelname)s %(funcName)s %(lineno)s %(asctime)s %(message)s',
        },
        'simple': {
            'format': '%(levelname)s %(message)s',
        },
    },
}
```

#### 3.定义日志的处理方式

```
'handlers': {
    'stu_handlers': {
        'level': 'DEBUG',
        # 指定日志写入文件的方式
        'class': 'logging.handlers.RotatingFileHandler',
        # 指定使用哪种格式来输出日志
        'formatter': 'verbose',
        # 指定日志文件存到那个文件中
        'filename': '%s/log.txt' % LOG_PATH,
        # 定义日志文件的大小为5M
        'maxByte': 5 * 1024 * 1024,
    },
},
```

#### 4.定义日志接口

```
'loggers': {
    'console': {
        # 指定logger的级别
        'level': 'INFO',
        # 指定使用'stu_handlers'处理日志
        'handlers': ['stu_handlers'],
        # propagate为True表示输出日志，并消息往更高级的地方传递，root为最高级，否则只输出日志，不传递消息
        'propagate': False,
    },
},
```

>注意：loggers的级别一定要比handlers的级别高。否则handlers会忽略掉该信息的。

#### 5.打印logging日志

以登录模块为例：

```
import logging

logger = logging.getLogger('console')

def login(request):
    if request.method == 'GET':
        logger.info('获取登录页面！')
        return render(request, 'day6_login.html')

    if request.method == 'POST':
        name = request.POST.get('name')
        password = request.POST.get('password')
        if UserAuth.objects.filter(a_user=name).exists():
            user = UserAuth.objects.filter(a_user=name).first()
            db_pass = user.a_pass
            if check_password(password, db_pass):
                ticket = ''
                s = '1234567890qwertyuiopasdfghjklzxcvbnm'
                for i in range(15):
                    ticket += random.choice(s)
                ticket += str(int(time.time()))
                user.a_ticket = ticket
                user.save()
                response = HttpResponseRedirect('/img/showimg/')
                response.set_cookie('ticket', ticket, max_age=3000)
                return response
            else:
                logger.error('登录失败')
                return render(request, 'day6_login.html')
        else:
            logger.error('登录失败')
            return render(request, 'day6_login.html')
```

日志记录结果：

![存储的日志记录](https://i.imgur.com/QkzmPqe.png)

>注意：在定义formatters/handlers/loggers的时候其中的关键字是固定的，在写的时候需要注意

参考自：
[http://blog.51cto.com/davidbj/1433741](http://blog.51cto.com/davidbj/1433741)
