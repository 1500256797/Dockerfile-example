#===============================================================================================================
#                        一、环境准备【必须】 
#===============================================================================================================
# windows系统安装docker desktop桌面端 网络环境一定要稳定 你懂得！
# 检查docker是否安装成功 docker version 或者 docker images
# 【注意！】不要在docker容器中安装使用docker和docker-compose服务。尝试了一天依旧行不通。所以就算有解决方案，那成本也很高昂。

#===============================================================================================================
#                        二、制作镜像(软件包)【必须】 
#===============================================================================================================
# 拉取nginx基础镜像
FROM nginx:1.21.1

# 维护者信息
MAINTAINER ouhuang

# 将dist文件中的内容复制到 `/usr/share/nginx/html/` 这个目录下面
COPY dist/  /usr/share/nginx/html/
# 用本地Docker/nginx目录下的nginx.conf 来替换nginx镜像里的默认配置
COPY Docker/nginx/nginx.conf /etc/nginx/nginx.conf

# 对外暴漏的端口号
# [注：EXPOSE指令只是声明容器运行时提供的服务端口，给读者看有哪些端口，在运行时只会开启程序自身的端口！！]
EXPOSE 80

# 启动nginx容器
CMD ["nginx", "-g", "daemon off;"]

#===============================================================================================================
#                        三、给Docker容器安装虚拟桌面，并设置支持vnc远程访问【可选】
#===============================================================================================================
# 1.设置环境变量
# 颜色位数16/24/32可用，越高画面越细腻，但网络不好的也会更卡
# ENV VNC_COL_DEPTH=24
# # 屏幕分辨率，800×600/1024×768诸如此类的可自己调整
# ENV VNC_RESOLUTION=1280x1024
# # VNC的服务端口号
# ENV VNC_PORT=5901
# # NOVNC的服务端口号
# ENV NOVNC_PORT=6080
# # 虚拟屏幕的编号
# ENV DISPLAY=:0
# # 2.安装xvfb模拟屏幕
# RUN apt-get update && apt-get install -y xvfb
# #  开启xvfb模拟屏幕  Xvfb :1 -ac -screen 0 960x540x24  开启一个序号为1的虚拟屏幕，分辨率为960x540，颜色位数为24
# #  Xvfb $DISPLAY -ac -screen 0 ${VNC_RESOLUTION}x${VNC_COL_DEPTH}  -listen tcp  -noreset &
# # 3.安装x11vnc 用于远程访问
# RUN apt-get install -y x11vnc
# # 运行x11vnc 屏幕共享  -display :1 指定虚拟屏幕的编号 -rfbport 5901 指定端口号  -scale 1280x1024 缩放屏幕 
# # -repeat 重复按键  -forever 一直运行  -shared 允许多个客户端连接  -bg 后台运行
# #  x11vnc -display $DISPLAY -rfbport $VNC_PORT -scale $VNC_RESOLUTION -repeat -shared -forever -bg

# # 4. 设置容器启动时执行的命令
# RUN echo '#!/bin/bash' >> /usr/local/app/start.sh
# RUN echo 'Xvfb $DISPLAY -ac -screen 0 ${VNC_RESOLUTION}x${VNC_COL_DEPTH}  -listen tcp  -noreset &' >> /usr/local/app/start.sh
# RUN echo 'x11vnc -display $DISPLAY -rfbport $VNC_PORT -scale $VNC_RESOLUTION -repeat -shared -forever -bg' >> /usr/local/app/start.sh
# RUN echo 'google-chrome --no-sandbox' >> /usr/local/app/start.sh  
# RUN chmod +x /usr/local/app/start.sh
# CMD ["/usr/local/app/start.sh"]
# 5. 使用vnc客户端访问 127.0.0.1:5901

#===============================================================================================================
#                        四、打包镜像【必须】 
#===============================================================================================================
#  --progress plain 不显示进度条  -t 镜像名:版本号  可以指定版本号/如果不指定版本号，默认为latest  
# docker image build --progress plain -t chatgpt:2.3 .

#  -f：指定Dockerfile文件路径 -t：镜像命名 --no-cache：构建镜像时不使用缓存  
# docker image build -f ./Docker/Dockerfile   --progress plain -t my-vue:2.0 . 

#===============================================================================================================
#                        五、启动容器并暴露端口【必须】 
#===============================================================================================================
#  --name 容器名 -p 5901:5901 5901端口映射到本机的5901端口  -p 6080:6080 6080端口映射到本机的6080端口 -t 镜像名  -i 交互式
# docker run --name chatgpt -p 5901:5901 -p 6080:6080 -i -t chatgpt:2.4
#  --name 容器名 -p 80:8091 容器的80端口映射到宿主机器的8091端口   -t 镜像名  -i 交互式
# docker run --name myvue2   -p 8091:80 --restart=always -i -t my-vue:2.0

#===============================================================================================================
#                        六、测试容器功能是否正常【必须】 
#===============================================================================================================
# 1、 测试vnc远程访问功能是否正常
# 使用vncviewer连接127.0.0.1:5901  
# 在docker 命令行中启动google浏览器          google-chrome-stable --no-sandbox
# 在vncviewer中能看到google浏览器则标识vnc远程访问功能正常




#===============================================================================================================
#                        七、docker其他小功能【可选】 
#===============================================================================================================

# # 更换国内阿里云yum源
# RUN cd /etc/yum.repos.d/
# RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
# RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
# RUN yum update -y
 
# # 安装netstat
# RUN yum install -y net-tools
# # 安装passwd 修改密码
# RUN yum install -y passwd
#  # 安装sshd
#  RUN yum install -y openssh-server openssh-clients
#  RUN sed -i '/^HostKey/'d /etc/ssh/sshd_config
#  RUN echo 'HostKey /etc/ssh/ssh_host_rsa_key'>>/etc/ssh/sshd_config
 
#  # 生成 ssh-key
#  RUN ssh-keygen -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key
 
#  # 更改 root 用户登录码为
#  RUN echo 'root:123456' | chpasswd
 
#  # 开发 22 端口
# EXPOSE 18630 3000 80 443 22

 
# # 镜像运行时启动sshd
# RUN mkdir -p /opt
# RUN echo '#!/bin/bash' >> /opt/run.sh
# RUN echo '/usr/sbin/sshd -D' >> /opt/run.sh
# RUN chmod +x /opt/run.sh
# CMD ["/opt/run.sh"]


#===============================================================================================================
#                        八、docker教程 
#===============================================================================================================
# https://blog.csdn.net/qq_41744950/article/details/124546973

# docker Centos 7 安装 xfce4 桌面 + x11vnc + novnc
# https://blog.csdn.net/qq_20919883/article/details/127704629