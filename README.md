# jupyter_notebook_pro

一个jupyter notebook的docker镜像， 对原版notebook进行了一些个人优化。

项目地址: [https://github.com/brokyz/jupyter_notebook_pro](https://github.com/brokyz/jupyter_notebook_pro)

## 优化内容

- 优化jupyter notebook的UI显示。

- 优化jupyter notebook对中文的支持。

- 优化了部分包对中文字体的显示乱码问题。

- 为jupyter notebook添加了Nbextensions功能增强插件。

- 支持python3。

- 优化终端对中文字体的显示。

- 对jupyter notebook的界面显示进行了美化。

## 下载镜像并搭建（推荐）

**拉取镜像**

```
docker pull brokyz/jupyter_notebook_pro
```

**部署镜像**

```
docker run -d -p 1000:8888 --name=jupyter_notebook_pro --restart=always brokyz/jupyter_notebook_pro

# 如果存在端口冲突，请修改端口。比如想要通过ip:9999访问，则需要改成-p 9999:8888
```

**登录并查看镜像部署日志获取token**

```
docker logs jupyter_notebook_pro

# 结果如下，token为1294c0bfc1513e9f2250df2ad52d40eb40b1ab678341e563

--------------------------------------------------------------------------------------------------------------------------------------------------

    To access the notebook, open this file in a browser:

        file:///root/.local/share/jupyter/runtime/nbserver-8-open.html

    Or copy and paste one of these URLs:

        http://b1c5c1eb1901:8888/?token=1294c0bfc1513e9f2250df2ad52d40eb40b1ab678341e563

     or http://127.0.0.1:8888/?token=1294c0bfc1513e9f2250df2ad52d40eb40b1ab678341e563
```

在浏览器输入 本机ip:1000

输入获得的token，并设置自己的登陆密码

登陆密码设置后，需要重启容器才可以生效

```
docker restart notebook_hub
```

之后仅需要通过 ip:1000 就可以访问了

## 使用Dockerfile创建镜像(适合带宽不高的机器)

**下载相关Dockerfile**

```
rm -rf build_notebook && mkdir build_notebook && wget -P build_notebook/ https://raw.githubusercontent.com/brokyz/jupyter_notebook_pro/main/Dockerfile && docker build -t brokyz/jupyter_notebook_pro:latest build_notebook/ && rm -rf build_notebook
```

**通过Dockerfile构建需要一定的时间, 镜像构建完成后执行上面的部署操作**

## 使用

**相关信息**

- jupyter notebook的相关配置文件位于`/root/.jupyter`目录

- jupyter notebook的工作目录位于`/root/notebook`目录

**修改工作目录**

修改配置文件/root/.jupyter/jupyter_notebook_config.py工作目录配置

```
c.NotebookApp.notebook_dir = '/root/notebook'
```

**安装python包**

在jupyter notebook中，可以在notebook的代码块中运行以下命令安装python包

```
!pip install numpy pandas matplotlib
```

在终端下直接使用`pip`安装即可

**添加对R语言的支持**

安装时间较长需要耐心等待...
```
apt install r-base -y && Rscript -e "install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))" && Rscript -e "install.packages('IRkernel')" && Rscript -e "IRkernel::installspec(user=FALSE)"
```

**安装R包**

在jupyter notebook中，可以在notebook的代码块中运行以下命令安装R包

```
install.packages('formatR')
```

在终端中可以使用以下代码安装R包

```
Rscript -e "install.packages('formatR')"
```

**修改相关字体大小，行间距等UI信息**

修改用户自定义css文件/root/.jupyter/custom/custom.css

默认优化后的配置为 [custom.css](https://github.com/brokyz/jupyter_notebook_pro/blob/main/config/custom.css)

**添加matplotlib显示字体**

这里自带了`SimHei`字体，可以使用以下代码来添加SimHei字体

```
import matplotlib.pyplot as plt

plt.rcParams['font.family']=['SimHei']

plt.rcParams['axes.unicode_minus']=False
```

如果需要其他字体，需要将字体包移动到`/usr/share/fonts`目录下，重启docker容器后生效
