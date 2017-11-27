# Example
> docker COMMAND [OPTION] IMAGE [COMMAND] [ARG...]
```
docker run ubuntu:14.04 /bin/echo 'Hello world'
```

* docker run
    > 指定了 docker 二进制执行文件和我们想要执行的命令 run。docker run 组合会运行容器
* ubuntu 14.04
    > 这是我们运行容器的来源。 Docker 称此为镜像。在本例中，我们使用一个 Ubuntu 14.04 操作系统镜像。当你指定一个镜像，Docker **首先会先从你的 Docker 本地主机上查看镜像是否存在**，如果没有，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
* /bin/echo 'Hello world'
    > 当我们的容器启动了 Docker 创建的新的 Ubuntu 14.04 环境，并在容器内执行 /bin/echo 命令后输出hello world

# 参数[OPTION]
* -t
    > -t 表示在新容器内指定一个伪终端或终端
* -i
    > -i表示允许我们对容器内的 (STDIN) 进行交互
* -d (守护进程)
    > -d 标识告诉 docker 在容器内以后台进程模式运行,并会输出容器ID（container ID）`1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147`   
    容器 ID 是有点长并且非常的笨拙，稍后我们会看到一个`短点的ID`,某些方面来说它是容器 ID 的简化版。
* -p
    > -p 标识通知 Docker 将容器内部使用的网络端口映射到我们使用的主机上。
* -P
    > -P 标识(flags)是 -p 5000 的缩写，它将会把容器内部的 5000 端口映射到本地 Docker 主机的高位端口上(这个端口的通常范围是 32768 至 61000)

# 退出容器
* exit 命令或者使用 CTRL-D 来退出容器
    > 与我们之前的容器一样，一旦你的 Bash shell 退出之后，你的容器就`停止`了
* container后台运行时(-d或docker start运行)通过`docker attach container_name`进入，并exit退出时container会`停止`
* container后台运行时(-d或docker start运行)通过`docker exec [option] container_name COMMAND [ARG...]`进入，并exit退出时container`不会停止`

# CAMMAND
* docker ps -a 查看容器
* docker logs container_name 命令会查看容器内的标准输出
* docker stop container_name 命令会通知 Docker 停止正在运行的容器。如果它成功了，它将返回刚刚停止的容器名称。
* docker start container_name 重启WEB应用容器
* docker rmi image_id 移除镜像
* docker rm container_name 移除WEB应用容器
* docker rm -f container_name 强制删除WEB应用容器即便正在运行

# 查看网络端口快捷方式
* docker port container_name 5000
    > docker port 可以查看指定 （ID或者名字的）容器的某个确定端口映射到宿主机的端口号。可以看到容器的 5000 端口映射到了宿主机的的端口号。

# 查看WEB应用程序日志
* docker logs -f container_name
    > docker log 命令就像使用 tail -f 一样来输出容器内部的标准输出。这里我们从显示屏上可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。

# 查看WEB应用程序容器的进程
* docker top container_name
    > 使用 `docker top` 来查看容器内部运行的进程

# 运行一个带标签镜像的容器
```
$ sudo docker run -t -i ubuntu:12.04 /bin/bash
```
如果你不指定一个镜像的版本标签，例如你只使用 Ubuntu，Docker 将默认使用 Ubuntu:latest 镜像。
> 提示：我们建议使用镜像时指定一个标签，例如 ubuntu:12.04 。这样你知道你使用的是一个什么版本的镜像。

# 获取一个新的镜像
> docker pull 命令来下载它

# 查找镜像
* docker search
    > 譬如说我们的团队需要一个安装了 Ruby 和 Sinatra 的镜像来做我们的 web 应用程序开发。我们可以通过 docker search 命令搜索 sinatra 来寻找适合我们的镜像

# 镜像分类
* 我们已经看到了两种类型的镜像，像ubuntu 镜像，我们称它为基础镜像或者根镜像。这些镜像是由 Docker 官方提供构建、验证和支持。这些镜像都可以通过用自己的名字来标记。  
* 我们也可以查找其它用户的公开镜像，像我们选择使用的 training/sinatra 镜像，这属于个人镜像。个人镜像是由 Docker 社区的成员创建和维护的。你可以用用户名称来识别这些镜像，因为这些镜像的前缀都是以用户名来标记的 ，像 training ，就是由 training 用户创建的镜像。

# 创建我们自己的镜像
团队成员发现 training/sinatra 镜像对于我们来说是非常有用的，但是它不能满足我们的需求，我们需要针对这个镜像做出更改。这里我们有两种方法，更新镜像或者创建一个新的镜像。
* 1.我们可以从已经创建的容器中更新镜像，并且提交这个镜像。
* 2.我们可以使用 Dockerfile 指令来创建一个新的镜像。

# 更新并提交镜像
```
docker commit -a="shiwan66" -m="add json gem " container_id shiwan66/sinatra:v2
```
* -m 
    > -m 标识我们指定提交的信息，就像你提交一个版本控制
* -a 
    > -a 标识允许对我们的更新来指定一个作者
* `shiwan66/sinatra:v2`创建的目标镜像
    > 先给这个镜像分配了一个新用户名字 `shiwan66`，接着，未修改镜像名称，保留了原有镜像名称`sinatra`；最后为镜像指定了标签(tag) `v2`

# 设置镜像标签
```
$ docker tag 5db5f8471261 ouruser/sinatra:devel
```
docker tag 需要使用镜像ID，这里是 5db5f8471261 ,用户名称、镜像源名(repository name)和新的标签名(tag)。docker

# 使用 Dockerfile 构建镜像
```docker
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```
* 让我们看一下我们的 Dockerfile 做了什么。每一个指令的前缀都必须是大写的。
* 我们可以使用 # 来注释
* 1.第一个指令 FROM 是告诉 Docker 使用的哪个镜像源，在这个案例中，我们使用的是 Ubuntu 14.04 基础镜像。
* 2.下一步，我们使用 MAINTAINER 指令来指定谁在维护这个新镜像。
* 3.最后，我们指定了两个 RUN 指令。 RUN 指令在镜像内执行一条命令，例如：安装一个包。这里我们更新了 APT 的缓存，并且安装 Ruby 和 RubyGems ，然后使用 gem 安装 Sinatra 。
```
$ docker build -t ouruser/sinatra:v2 .
```
我们使用 docker build 命令并指定 -t 标识(flag)来标示属于 ouruser ，镜像名称为 sinatra,标签是 v2。   
如果 Dockerfile 在我们当前目录下，我们可以使用 . 来指定 Dockerfile
现在我们可以看到构建过程。Docker 做的第一件事是通过你的上下文进行构建，基本上是目录的内容构建。这样做是因为 Docker 进程构建镜像是实时构建的，并且是需要本地的上下文来做这些工作的。（这里上下文是指Context）  
下一步，Dockerfile 中的每一条命令都一步一步的被执行。我们会看到`每一步都会创建一个新的容器`，在`容器内部运行指令并且提交更改` - 就像我们之前使用的 docker commit 一样。当所有的指令执行完成之后，我们会得到97feabe5d2ed 镜像（也帮助标记为 ouruser/sinatra:v2）, 然后`所有中间容器会被清除`。

# 网络端口映射
我们还有很多设置-p标识的方法。默认-p标识会绑定本地主机上的指定端口。并且我们可以指定绑定的网络地址。举例设置localhost
```
$ sudo docker run -d -p 127.0.0.1:5001:5002 training/webapp python app.py
```
这将绑定容器内部5002端口到主机的localhost或者127.0.0.1的5001端口。

如果要绑定容器端口5002到宿主机动态端口，并且让localhost访问，我们可以这样做：
```
$ sudo docker run -d -p 127.0.0.1::5002 training/webapp python app.py
```
我们可以使用docker port快捷方式来绑定我们的端口，这有助于向我们展示特定的端口。例如我们绑定localhost,如下是docker port输出：
```
$ docker port nostalgic_morse 5000
127.0.0.1:49155
```
我们也可以绑定UDP端口，我们可以在后面添加/udp,举例：
```
$ sudo docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
```

# 容器命名
* 1.给容器特定的名字使你更容易记住他们，例如：命名web应用程序为web容器。
* 2.它为docker提供一个参考，允许其他容器引用，举例连接web容器到db容器。
```
$ sudo docker run -d -P --name web training/webapp python app.py
```
我们也可以使用docker inspect来返回容器名字。
```
$ sudo docker inspect -f "{{ .Name }}" aed84ee21bde
/web
```
> 注：容器的名称必须是唯一的。这意味着你只能调用一个web容器。如果你想使用重复的名称来命名容器，你需要使用docker rm命令删除以前的容器。在容器停止后删除。

# 容器连接
连接允许容器之间可见并且安全地进行通信。使用--link创建连接。我们创建一个新容器，这个容器是数据库。
```
$ sudo docker run -d --name db training/postgres
```
这里我们使用training/postgres容器创建一个新的容器。容器是PostgreSQL数据库。  

现在我们创建一个web容器来连接db容器。
```
$ sudo docker run -d -P --name web --link db:db training/webapp python app.py
```
这将使我们的web容器和db容器连接起来。--link的形式
```
--link name:alias
```
`name`是我们连接容器的名字，alias是link的别名。让我们看如何使用alias。

让我们使用docker ps来查看容器连接.

# 数据卷
* 数据卷可在容器之间共享或重用
* 数据卷中的更改可以直接生效
* 数据卷中的更改不会包含在镜像的更新中
* 数据卷的生命周期一直持续到没有容器使用它为止
## 添加一个数据卷
你可以在docker run命令中使用-v标识来给容器内添加一个数据卷，你也可以在一次docker run命令中多次使用-v标识挂载多个数据卷。现在我们在web容器应用中创建单个数据卷。
```
$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```
这会在容器内部创建一个新的卷/webapp

> 注：类似的，你可以在Dockerfile中使用VOLUME指令来给创建的镜像添加一个或多个数据卷。
##  挂载一个主机目录作为卷
可以挂载本地主机目录到容器中：
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
```
这将会把本地目录/src/webapp挂载到容器的/opt/webapp目录。宿主机上的目录必须是绝对路径，如果目录不存在docker会自动创建它。
> 注：出于可移植和分享的考虑，这种方法不能够直接在Dockerfile中实现。作为宿主机目录——其性质——是依赖于特定宿主机的，并不能够保证在所有的宿主机上都存在这样的特定目录。  
docker默认情况下是对数据卷有读写权限，但是我们通过这样的方式让数据卷只读：
```
$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro training/webapp python app.py
```
这里我们同样挂载了`/src/webapp`目录，只是添加了ro选项来限制它只读。

# 将宿主机上的特定文件挂载为数据卷
除了能挂载目录外，-v标识还可以将宿主机的一个特定文件挂载为数据卷：
```
$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
```
上述命令会在容器中运行一个bash shell，当你退出此容器时在主机上也能够看到容器中bash的命令历史。