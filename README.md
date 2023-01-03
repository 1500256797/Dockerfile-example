# Dockerfile最佳实践

## Docker容器+虚拟屏幕Xfvb+远程桌面X11VNC+谷歌浏览器

### 使用场景：
一般linux系统是没有图形化界面的，如果你的程序需要图形化桌面下运行（例如谷歌浏览器），你又不想安装一个xfce或者使用windows server的话。你可以给linux安装一个虚拟屏幕，安装虚拟屏幕xfvb。但是只安装虚拟屏幕是无法调试的，这时你可以给docker容器安装VNC，当你的浏览器自动化脚本程序发生异常时，这样你随时就可以远程到虚拟桌面去调试。

### 效果图
- 未启动浏览器时的虚拟桌面，屏幕黑漆漆的。
![image](img/no-chrome.png)

- 使用`google-chrome --no-sandbox`命令启动浏览器后，可以看到虚拟桌面上的浏览器。
![image](img/chrome.png)