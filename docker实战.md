### 一 、Docker的介绍

* Docker时Docker.Lnc公司开源的一个基于LXC技术之上搭建的Container容器引擎，源代码托管在Github上

*  基于Go语言并遵从Apache2.0协议开源。

* Docker属于Linux容器的一种封装，提供简单易用的容器使用接口。

* Docker将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了Docker，就不用担心环境问题。

* 总体来说，Docker的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

  

### 二、 Docker的概念

> ```scheme
> Docker是开发人员和系统管理员使用容器开发、部署和运行应用程序的平台。使用Linux容器来部署应用程序称为集装箱化。使用docker轻松部署应用程序。
> ```

**集装箱化的优点：**

- 灵活：即使是复杂的应用程序也可封装。
- 轻量级：容器利用并共享主机内核。
- 便携式：您可以在本地构建，部署到云上并在任何地方运行。
- 可扩展性：您可以增加和自动分发容器副本。
- 可堆叠：您可以垂直堆叠服务并及时并及时堆叠服务。

**采用 images 和容器**

> ```scheme
> 通过运行images启动容器，一个images是一个可执行的包，其中包括运行应用程序所需要的所有内容-代码，运行时，库、环境变量和配置文件。
> 容器时images运行时示例-当被执行时(即，images状态，或者用户进程)在内存中，可以使用命令查看正在运行容器的列表docker ps,就像在Linux中一样。
> ```

***对比传统技术： 虚拟机***

>```scheme
>虚拟机(virtual machine)就是带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在Windows系统里面运行Linux系统。应用程序对此毫无感知，因为虚拟机看上去跟真丝系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其它部分毫无影响。
>```

**虚拟机的缺点：**

- **资源占用多：**虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了。哪怕虚拟机里面的应用程序，真正使用的内存只有1M，虚拟机依然需要几百MB的内容才能运行。
- **冗余步骤多：**虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。
- **启动慢：**启动操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，应用陈故乡才能真正运行。

**linux容器**

> ```scheme
> 由于虚拟机存在这个缺点，Linux发展出了另一种虚拟化技术：Linux容器(Linux Containers,缩写为LXC)。
> Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。由于容器是进程级别的，相比虚拟机又很多优势。
> ```

- **启动快：**容器里面的应用，直接就是底层系统的一个进程，而不是虚拟机内部的进程。所以，启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多。

- **资源占用少：**容器只占用需要的资源，不占用那些没有用到的资源；虚拟机由于是完整的操作系统，不可避免要占用所以资源。另外，多个容器可以共享资源，虚拟机都是独享资源。

- 体积小：容器只要包含用到的组件即可，而虚拟机是整个操作系统的打包，所以容器文件比虚拟机文件要小很多。总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

  

  ![image-20210628160853119](pic/image-20210628160853119.png)



### 三、Docker安装

* 环境准备

  * Deepin

* 环境查看

  ~~~ shell
  # 系统内核 5.10以上
  rison@rison-PC:~/Desktop$ uname -r
  5.10.29-amd64-desktop
  (base) rison@rison-PC:~/Desktop$ cat /etc/os-release 
  PRETTY_NAME="Deepin 20.2.1"
  NAME="Deepin"
  VERSION_ID="20.2.1"
  VERSION="20.2.1"
  ID=Deepin
  HOME_URL="https://www.deepin.org/"
  BUG_REPORT_URL="https://bbs.deepin.org/"
  ~~~

  

* 安装 

[docker官网](https://docs.docker.com/)

![image-20210628162446190](pic/image-20210628162446190.png)

**帮助文档**

~~~shell
#1、卸载旧的版本
 sudo apt-get remove docker docker-engine docker.io containerd runc
 
#2、更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：
 sudo apt-get update
 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
#3、添加docker官方的秘钥
 curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 
#4、设置稳定的存储库
 echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
#5、安装docker引擎
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
 
#6、测试是否安装成功
(base) rison@rison-PC:~/Desktop$ docker --version
Docker version 19.03.8, build 1b4342cd4c
(base) rison@rison-PC:~/Desktop$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:9f6ad537c5132bcce57f7a0a20e317228d382c3cd61edae14650eec68b2b345c
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

~~~

### 四、docke运行流程

![image-20210628164530931](pic/image-20210628164530931.png)

**docker工作原理**

Docker是一个client-server结构的系统，Docker的守护进程运行在主机上，通过socket从客户端访问！

DockerServer接收到DockerClient的指令，就会执行这个命令！

![image-20210628165214919](pic/image-20210628165214919.png)

### 五、docker常用命令

~~~shell
rison@rison-PC:~/Desktop$ docker version  #显示版本信息
rison@rison-PC:~/Desktop$ docker info #显示系统信息，包括镜像和容器的数量
rison@rison-PC:~/Desktop$ docker --help #帮助命令
~~~

**镜像命令**

~~~shell
rison@rison-PC:~/Desktop$ docker images # 查看所有本地主机的镜像
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              d1165f221234        3 months ago        13.3kB

# 解释
REPOSITORY # 镜像的仓库源
TAG #镜像版本
IMAGE ID #镜像的ID
CREATED #镜像的创建时间
SIZE #镜像的大小

# 可选项
Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs


# docker search 搜索镜像
rison@rison-PC:~/Desktop$ docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11057               [OK]                
mariadb                           MariaDB Server is a high performing open sou…   4193       
# 可选项
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output

--filter=STARS=3000

rison@rison-PC:~/Desktop$ docker search mysql --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   11057               [OK]                
mariadb             MariaDB Server is a high performing open sou…   4193                [OK] 

# docker pull 下载镜像 默认是最新的版本可以在后面添加 [:tag]来指定版本
rison@rison-PC:~/Desktop$ docker pull mysql 
Using default tag: latest
latest: Pulling from library/mysql
b4d181a07f80: Pull complete 
a462b60610f5: Pull complete 
578fafb77ab8: Pull complete 
524046006037: Pull complete 
d0cbe54c8855: Pull complete 
aa18e05cc46d: Pull complete 
32ca814c833f: Pull complete 
9ecc8abdb7f5: Pull complete 
ad042b682e0f: Pull complete 
71d327c6bb78: Pull complete 
165d1d10a3fa: Pull complete 
2f40c47d0626: Pull complete 
Digest: sha256:52b8406e4c32b8cf0557f1b74517e14c5393aff5cf0384eff62d9e81f4985d4b
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest

# 之前存在的可以复用，分层概念
rison@rison-PC:~/Desktop$ docker pull mysql:5.7
5.7: Pulling from library/mysql
b4d181a07f80: Already exists 
a462b60610f5: Already exists 
578fafb77ab8: Already exists 
524046006037: Already exists 
d0cbe54c8855: Already exists 
aa18e05cc46d: Already exists 
32ca814c833f: Already exists 
52645b4af634: Pull complete 
bca6a5b14385: Pull complete 
309f36297c75: Pull complete 
7d75cacde0f8: Pull complete 
Digest: sha256:1a2f9cd257e75cc80e9118b303d1648366bc2049101449bf2c8d82b022ea86b7
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7


#删除镜像
rison@rison-PC:~/Desktop$ docker rmi -f 09361feeb475
Untagged: mysql:5.7
Untagged: mysql@sha256:1a2f9cd257e75cc80e9118b303d1648366bc2049101449bf2c8d82b022ea86b7
Deleted: sha256:09361feeb4753ac9da80ead4d46e2b21247712c13c9ee3f1e5d55630c64c544f
Deleted: sha256:e454d1e47d2f346e0b2365c612cb6f12476ac4a3568ad5f62d96aa15bccf3e19
Deleted: sha256:e0457c6e331916c8ac6838ef4b22a6f62b21698facf4e143aa4b3863f08cf7d2
Deleted: sha256:ed73046ee2cd915c08ed37a545e1b89da70dc9bafeacfbd9fddff8f967373941
Deleted: sha256:419d7a76abf4ca51b81821da16a6c8ca6b59d02a0f95598a2605a1ed77c012eb

#全部删除
rison@rison-PC:~/Desktop$ docker rmi -f $(docker images -aq)

~~~

**容器命令**

* 有了镜像才能创建容器

~~~shell
rison@rison-PC:~/Desktop$ docker pull centos
Using default tag: latest
latest: Pulling from library/centos
7a0437f04f83: Pull complete 
Digest: sha256:5528e8b1b1719d34604c87e11dcd1c0a20bedf46e83b5632cdeac91b8c04efc1
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest

~~~

* 新建容器并启动

~~~shell
docker run [可选参数] image
# 参数说明
--name="Name" 容器的名字，用来区分容器名称
-d 后台运行
-it 使用交互方式运行，进入容器查看内容
-P 指定容器的端口 -p 8080:80
   -p 主机端口：容器端口
   -p 容器端口
-P 大写P随机指定端口

# 退出容器
exit
~~~

![image-20210628174142304](pic/image-20210628174142304.png)

* 列出所有的运行容器

  ~~~shell
  # docker ps 
  Options:
    -a, --all             Show all containers (default shows just running)
    -f, --filter filter   Filter output based on conditions provided
        --format string   Pretty-print containers using a Go template
    -n, --last int        Show n last created containers (includes all states) (default -1)
    -l, --latest          Show the latest created container (includes all states)
        --no-trunc        Don't truncate output
    -q, --quiet           Only display numeric IDs
    -s, --size            Display total file sizes
  
  (base) rison@rison-PC:~/Desktop$ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  (base) rison@rison-PC:~/Desktop$ docker ps -a
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
  ed8c9e118057        centos              "/bin/bash"         4 minutes ago       Exited (127) 59 seconds ago                        boring_rhodes
  c5c221dfa0ba        d1165f221234        "/hello"            About an hour ago   Exited (0) About an hour ago                       epic_ride
  (base) rison@rison-PC:~/Desktop$ 
  ~~~

  

* 退出容器

  ~~~shell
  exit 退出容器
  ctrl + P + Q 退出不停止
  ~~~

  

* 删除容器

  ~~~ shell
  docker rm #容器id,不能删除正在运行的容器，可以改为 docker rm -f 可以删除
  docker rm -f $(docker ps -aq) #删除所有容器
  docker ps -a -q | xargs docker rm #删除所有容器
  
  ~~~

* 启动和停止容器操作

  ~~~shell
  docker start 容器id #启动容器
  docker restart 容器id #重启容器
  docker stop 容器id #停止容器
  docker kill 容器id # 强制停止
  ~~~

  

* 常用其他命令

  ~~~shell
  # 命令docker run -d 镜像名
  rison@rison-PC:~/Desktop$ docker run -d centos
  # 发现docker ps ,停止了
  # 描述： 容器使用嗯后台运行，必须要一个前台应用，否则会自动停止。容器启动后，发现没有提供服务，就会自动停止。
  ~~~

* 查看日志

  ~~~shell
  rison@rison-PC:~/Desktop$ docker logs -f -t --tail 100 695b14b0ec01
  ~~~

* 查看容器中的进程信息

  ```shell
  # 命令 docker top 容器id
   rison@rison-PC:~/Desktop$ docker top b7e008764fb3
  ```

  ![image-20210628181126765](pic/image-20210628181126765.png)

* 查看容器的信息

  ~~~shell
  (base) rison@rison-PC:~/Desktop$ docker inspect b7e008764fb3
  [
      {
          "Id": "b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03",
          "Created": "2021-06-28T10:10:26.805429488Z",
          "Path": "/bin/bash",
          "Args": [],
          "State": {
              "Status": "running",
              "Running": true,
              "Paused": false,
              "Restarting": false,
              "OOMKilled": false,
              "Dead": false,
              "Pid": 6094,
              "ExitCode": 0,
              "Error": "",
              "StartedAt": "2021-06-28T10:10:27.094616334Z",
              "FinishedAt": "0001-01-01T00:00:00Z"
          },
          "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
          "ResolvConfPath": "/var/lib/docker/containers/b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03/resolv.conf",
          "HostnamePath": "/var/lib/docker/containers/b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03/hostname",
          "HostsPath": "/var/lib/docker/containers/b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03/hosts",
          "LogPath": "/var/lib/docker/containers/b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03/b7e008764fb38ccbb0cd0d3df55e1f3b1638642e625941ced0b1fd649ff31a03-json.log",
          "Name": "/zen_driscoll",
          "RestartCount": 0,
          "Driver": "overlay2",
          "Platform": "linux",
          "MountLabel": "",
          "ProcessLabel": "",
          "AppArmorProfile": "docker-default",
          "ExecIDs": null,
          "HostConfig": {
              "Binds": null,
              "ContainerIDFile": "",
              "LogConfig": {
                  "Type": "json-file",
                  "Config": {}
              },
              "NetworkMode": "default",
              "PortBindings": {},
              "RestartPolicy": {
                  "Name": "no",
                  "MaximumRetryCount": 0
              },
              "AutoRemove": false,
              "VolumeDriver": "",
              "VolumesFrom": null,
              "CapAdd": null,
              "CapDrop": null,
              "Capabilities": null,
              "Dns": [],
              "DnsOptions": [],
              "DnsSearch": [],
              "ExtraHosts": null,
              "GroupAdd": null,
              "IpcMode": "private",
              "Cgroup": "",
              "Links": null,
              "OomScoreAdj": 0,
              "PidMode": "",
              "Privileged": false,
              "PublishAllPorts": false,
              "ReadonlyRootfs": false,
              "SecurityOpt": null,
              "UTSMode": "",
              "UsernsMode": "",
              "ShmSize": 67108864,
              "Runtime": "runc",
              "ConsoleSize": [
                  0,
                  0
              ],
              "Isolation": "",
              "CpuShares": 0,
              "Memory": 0,
              "NanoCpus": 0,
              "CgroupParent": "",
              "BlkioWeight": 0,
              "BlkioWeightDevice": [],
              "BlkioDeviceReadBps": null,
              "BlkioDeviceWriteBps": null,
              "BlkioDeviceReadIOps": null,
              "BlkioDeviceWriteIOps": null,
              "CpuPeriod": 0,
              "CpuQuota": 0,
              "CpuRealtimePeriod": 0,
              "CpuRealtimeRuntime": 0,
              "CpusetCpus": "",
              "CpusetMems": "",
              "Devices": [],
              "DeviceCgroupRules": null,
              "DeviceRequests": null,
              "KernelMemory": 0,
              "KernelMemoryTCP": 0,
              "MemoryReservation": 0,
              "MemorySwap": 0,
              "MemorySwappiness": null,
              "OomKillDisable": false,
              "PidsLimit": null,
              "Ulimits": null,
              "CpuCount": 0,
              "CpuPercent": 0,
              "IOMaximumIOps": 0,
              "IOMaximumBandwidth": 0,
              "MaskedPaths": [
                  "/proc/asound",
                  "/proc/acpi",
                  "/proc/kcore",
                  "/proc/keys",
                  "/proc/latency_stats",
                  "/proc/timer_list",
                  "/proc/timer_stats",
                  "/proc/sched_debug",
                  "/proc/scsi",
                  "/sys/firmware"
              ],
              "ReadonlyPaths": [
                  "/proc/bus",
                  "/proc/fs",
                  "/proc/irq",
                  "/proc/sys",
                  "/proc/sysrq-trigger"
              ]
          },
          "GraphDriver": {
              "Data": {
                  "LowerDir": "/var/lib/docker/overlay2/f0f7f56f9ce2d850153f120c691e3d2ee57629746e0daeedd19f2e71dc986842-init/diff:/var/lib/docker/overlay2/6b1dd3a7aeaf26e9f7dd57ee907b58ebcf4adb734e2e62533a372cd7075d15b1/diff",
                  "MergedDir": "/var/lib/docker/overlay2/f0f7f56f9ce2d850153f120c691e3d2ee57629746e0daeedd19f2e71dc986842/merged",
                  "UpperDir": "/var/lib/docker/overlay2/f0f7f56f9ce2d850153f120c691e3d2ee57629746e0daeedd19f2e71dc986842/diff",
                  "WorkDir": "/var/lib/docker/overlay2/f0f7f56f9ce2d850153f120c691e3d2ee57629746e0daeedd19f2e71dc986842/work"
              },
              "Name": "overlay2"
          },
          "Mounts": [],
          "Config": {
              "Hostname": "b7e008764fb3",
              "Domainname": "",
              "User": "",
              "AttachStdin": true,
              "AttachStdout": true,
              "AttachStderr": true,
              "Tty": true,
              "OpenStdin": true,
              "StdinOnce": true,
              "Env": [
                  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
              ],
              "Cmd": [
                  "/bin/bash"
              ],
              "Image": "centos",
              "Volumes": null,
              "WorkingDir": "",
              "Entrypoint": null,
              "OnBuild": null,
              "Labels": {
                  "org.label-schema.build-date": "20201204",
                  "org.label-schema.license": "GPLv2",
                  "org.label-schema.name": "CentOS Base Image",
                  "org.label-schema.schema-version": "1.0",
                  "org.label-schema.vendor": "CentOS"
              }
          },
          "NetworkSettings": {
              "Bridge": "",
              "SandboxID": "d5d0893aea492de9f01eebd40d9b0532e79dc341e90f6fc47d43deea67d1e662",
              "HairpinMode": false,
              "LinkLocalIPv6Address": "",
              "LinkLocalIPv6PrefixLen": 0,
              "Ports": {},
              "SandboxKey": "/var/run/docker/netns/d5d0893aea49",
              "SecondaryIPAddresses": null,
              "SecondaryIPv6Addresses": null,
              "EndpointID": "f854c69be626303325b71e010e6b3c67a4c47f59872a8c1a829f0e89553a2c93",
              "Gateway": "172.17.0.1",
              "GlobalIPv6Address": "",
              "GlobalIPv6PrefixLen": 0,
              "IPAddress": "172.17.0.2",
              "IPPrefixLen": 16,
              "IPv6Gateway": "",
              "MacAddress": "02:42:ac:11:00:02",
              "Networks": {
                  "bridge": {
                      "IPAMConfig": null,
                      "Links": null,
                      "Aliases": null,
                      "NetworkID": "95f6bbb256f5d5c10e2a101d977bd347509df8b2cd26fa47eadf25f747cdcb9e",
                      "EndpointID": "f854c69be626303325b71e010e6b3c67a4c47f59872a8c1a829f0e89553a2c93",
                      "Gateway": "172.17.0.1",
                      "IPAddress": "172.17.0.2",
                      "IPPrefixLen": 16,
                      "IPv6Gateway": "",
                      "GlobalIPv6Address": "",
                      "GlobalIPv6PrefixLen": 0,
                      "MacAddress": "02:42:ac:11:00:02",
                      "DriverOpts": null
                  }
              }
          }
      }
  ]
  
  ~~~

* 进入当前正在运行的命令

  ```shell
  # 我们通常容器都是使用后台运行的，需要进入容器修改一些配置
  # 命令
  docker exec -it 容器id bashShell
  
  (base) rison@rison-PC:~/Desktop$ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  b7e008764fb3        centos              "/bin/bash"         5 minutes ago       Up 5 minutes                            zen_driscoll
  (base) rison@rison-PC:~/Desktop$ docker exec -it b7e008764fb3 /bin/bash
  [root@b7e008764fb3 /]# 
  
  
  #方式2 
  docker attach 容器id
  
  (base) rison@rison-PC:~/Desktop$ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  b7e008764fb3        centos              "/bin/bash"         7 minutes ago       Up 7 minutes                            zen_driscoll
  (base) rison@rison-PC:~/Desktop$ docker attach b7e008764fb3
  [root@b7e008764fb3 /]# 
  
  
  #docker exec 进入容器后开启一个新的终端，可以在里面操作（常用）
  #docker attach 进入容器正在执行终端，不会启用新的
  ```

  ![image-20210628182745434](image-20210628182745434.png)

![image-20210628183007917](cimage-20210628183007917.png)