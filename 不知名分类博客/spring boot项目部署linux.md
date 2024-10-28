# spring boot项目部署linux

## 前提

**Linux 服务器运维管理面板**：宝塔

**服务器系统：**ubantu

**开放端口：**服务器开放30158访问端口

外网面板地址: https://118.178.133.110:30158/75261c53
 		内网面板地址: https://172.22.112.27:30158/75261c53

## 本地测试

1. 从idea中打jar包去目录中用命令行执行

2. 开启redis
3. 用postman测试一下接口，能用说明jar包没有问题，修改一下配置就可以部署到服务器上了。

## 云服务器设置

1. 把jar包和yml配置文件上传至根目录下home的Java文件夹中

**外部的配置文件具有更高的优先级**，因为它们可以覆盖内部的配置文件。所以，如果你在云服务器上同时上传了JAR文件和外部的`.yml`文件，并且在启动应用程序时指定了外部配置文件的路径，那么外部配置文件中的配置会优先生效。

2. 安装redis后(自动运行)查看redis运行状态：

```bash
/etc/init.d/redis status       查看运行状态
/etc/init.d/redis start          启动
/etc/init.d/redis stop     停止服务
/etc/init.d/redis restart  重启服务
redis-cli info server 输出Redis服务器的一些基本信息，包括服务器监听的端口号	
```

3. 在云服务器中安装jdk环境（宝塔下一个jdk管理器）并配置对应的path路径：centos7需要在~/.bash_profile文件中添加路径，用vi编辑器打开。按住insert进入编辑模式。同时按住esc和冒号键输入wq进行保存并退出。使修改生效，可以重新加载 `~/.bash_profile` 文件：`source ~/.bash_profile`。搞定后可以使用`Java -version`测试。当你下次登录到 CentOS 7 时，Java 的路径就会自动添加到 PATH 中了，你可以直接在命令行中使用 `java` 命令了。
4. 设置云服务器数据库，导入本地数据库结构，设置数据库名和密码。在云服务器终端输入`mysql -u root -p'123456'`测试是否初始化成功。
5. 手动去初始化MySQL，不要用宝塔面板的操作。
6. 项目配置文件要设置对http的访问。
7. 服务器要开放项目监听的端口8080



