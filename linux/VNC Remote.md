# Centos 安装vnc / vncserver

### 1 安装

1. 以`root`用户运行以下命令来安装`vncserver`

   `yum install tigervnc-server `

2. 同样运行以下命令来安装`vncviewer`

   `yum install vnc`

3. 停止并禁用防火墙；

   `systemctl stop firewalld.service`

   `systemctl disable firewalld.service`

### 2 配置

1. `vncviewer`基本上不用配置；

2. `vncserver`的配置，创建一个新的配置文件，以开启1号窗口为例（也可以同时开启多个窗口，修改数字即可），方法如下：

   ```shell
   cp /lib/systemd/system/vncserver@.service /lib/systemd/system/vncserver@:1.service
   ```

   或者再增加一个窗口：
   ```shell
   cp /lib/systemd/system/vncserver@.service /lib/systemd/system/vncserver@:2.service
   ```

3. 编辑`/lib/systemd/system/vncserver@:1.service`,设置用户`root`相关参数,最终内容如下

   ```shell
   [Unit]
   Description=Remote desktop service (VNC)
   After=syslog.target network.target
   [Service]
   Type=forking

   # Clean any existing files in /tmp/.X11-unix environment
   ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
   ExecStart=/sbin/runuser -l root -c "/usr/bin/vncserver %i"
   PIDFile=/root/.vnc/%H%i.pid
   ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
   ```

　　上述内容中最好设置为`root`用户，否则可能会看到以下报错：


   ```shell
vncserver@:1.service - Remote desktop service (VNC)
   Loaded: loaded (/usr/lib/systemd/system/vncserver@:1.service; enabled)
   Active: failed (Result: exit-code) since Tue 2015-04-14 10:09:24 CST; 1min 36s ago
  Process: 3258 ExecStart=/sbin/runuser -l sysadmin -c /usr/bin/vncserver %i (code=exited, status=1/FAILURE)
  Process: 3254 ExecStartPre=/bin/sh -c /usr/bin/vncserver -kill %i > /dev/null 2>&1 || : (code=exited, status=0/SUCCESS)
Apr 14 10:09:24 F1A-VMHOST-SWPE systemd[1]: Starting Remote desktop service (VNC)...
Apr 14 10:09:24 F1A-VMHOST-SWPE runuser[3258]: Password:
Apr 14 10:09:24 F1A-VMHOST-SWPE systemd[1]: vncserver@:1.service: control process exited, code=exited status=1
Apr 14 10:09:24 F1A-VMHOST-SWPE systemd[1]: Failed to start Remote desktop service (VNC).
Apr 14 10:09:24 F1A-VMHOST-SWPE systemd[1]: Unit vncserver@:1.service entered failed state.
Warning: Unit file changed on disk, 'systemctl daemon-reload' recommended.
   ```


### 3 应用

1. 更新`systemctl`以使其生效；

   `systemctl daemon-reload `

2. 设置`vncserver`的密码；

    `vncpasswd root`

   按提示输入密码以及确认密码

3. 启动该服务用来启用`vnc`的1号窗口；

   ```shell
   systemctl start vncserver@:1.service  
   # 或者
   vncserver :1 
   ```

   关闭1号窗口：

   ```shell
   systemctl stop vncserver@:1.service   
   # 或者 
   vncserver -kill :1
   ```

4. 设置为开机自动启动；
   ```
   systemctl enable vncserver@:1.service
   ```


