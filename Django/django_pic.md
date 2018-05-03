django上传图片

## 上传图片

#### 前言：

我们在进行网站注册或者说大家用的最多的朋友圈，都有上传图片，或者头像的功能，这里我们来介绍如何上传图片。

#### 数据库相关配置：

创建一个APP叫做img，配置数据库连接，并创建存储图片和图片描述信息的表ImgUpload。

```
class ImgUpload(models.Model):
    i_desc = models.CharField(max_length=50)
    i_img = models.ImageField(upload_to='update', null= True)

    class Meta:
        db_table = 'img'
```

>注意：这里的upload_to用来指定图片的存储文件夹的名称，在上传文件之后如果该文件夹不存在会自动创建。

#### 修改项目配置文件

指定媒体文件的路径和根，在项目目录下创建media文件夹，同时在settings.py文件最后添加内容：

```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

#### 创建上传和显示图片的html静态文件

显示最后上传的图片和描述（showimg.html）：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>显示添加的图片</title>
</head>
<body>
<img src='/media/{{ lastimg.i_img }}'>
<p>{{ lastimg.i_desc }}</p>
</body>
</html>
```

创建上传图片的页面静态文件（uploadimg.html）:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上传一张图片</title>
</head>
<body>
<form method="post" action="" enctype="multipart/form-data">
    <p>图片描述：<input type="text" name="desc"></p>
    <p>图片：<input type="file" name="img"></p>
    <input type="submit" value="提交">
    <input type="reset" value="重置">

</form>


</body>
</html>
```

>注意：因为这里需要上传文件，所以表单提交的时候应该加上enctype="multipart/form-data"。


#### 创建上传和显示的视图函数

编辑APP目录下的views.py文件：

```
from django.http import HttpResponseRedirect
from django.shortcuts import render
from img.models import ImgUpload


# Create your views here.

def showimg(request):
    lastimg = ImgUpload.objects.last()
    return render(request, 'showimg.html', {'lastimg': lastimg})


def up_img(request):
    if request.method == 'GET':
        return render(request, 'uploadimg.html')
    if request.method == 'POST':
        img = request.FILES.get('img')
        desc = request.POST.get('desc')
        ImgUpload.objects.create(
            i_desc=desc,
            i_img=img,
        )

        return HttpResponseRedirect('/img/showimg/')
```

>注意：这里要获取到图片文件应该使用request.FILES.get()方法，使用request.POST.get()不能获取到提交的文件。

#### 配置url路由

编辑APP目录下的urls.py文件：
```
from django.conf.urls import url
from img import views

urlpatterns =[
    url(r'showimg/', views.showimg),
    url(r'uploadimg/', views.up_img),
]
```

编辑项目目录下的urls.py文件：
```
from django.conf.urls import url, include
from django.contrib import admin
from uploadimg import settings
from django.conf.urls.static import static

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^img/', include('img.urls', namespace='img'))
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

>注意：

>1.上传上来的图片文件应该放在静态目录下，但是我们在项目目录下创建的media目录并不是静态目录，所有我们需要将其装换为静态目录并添加到url路由中上传文件才能成功，装换方法为```urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)```

>2.如果media目录不是在项目目录下的，而是在static目录下，那么我们就不需要再项目目录中写```urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)```。只需要将settings.py中MEDIA_URL和MEDIA_ROOT改为```MEDIA_URL = '/static/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'static/media')```，同时修改图片的src路径为/static/media/{{ lastimg.i_path }}即可。

>3.上传上来的文件夹存储在指定的media目录下的upload目录下。