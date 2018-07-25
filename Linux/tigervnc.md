# RHEL7设置tigervnc

## 图形化
如操作系统未安装图形，则使用下面的命令安装图形。

    # yum groupinstall -y "Server with GUI"

## 安装tigervnc

    # yum install -y tigervnc-server

## 配置tigervnc

使用5903作为vnc的监听端口

    # cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:3.service
打开/etc/systemd/system/vncserver@:3.service文件

    [Unit]
    Description=Remote desktop service (VNC)
    After=syslog.target network.target
    
    [Service]
    Type=forking
    
    # Clean any existing files in /tmp/.X11-unix environment
    ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
    ExecStart=/usr/sbin/runuser -l root -c "/usr/bin/vncserver %i"
    PIDFile=/root/.vnc/%H%i.pid
    ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
    
    [Install]
    WantedBy=multi-user.target
修改用户名

ExecStart=/usr/sbin/runuser -l **root** -c "/usr/bin/vncserver %i"

PIDFile=/**root**/.vnc/%H%i.pid
## 设置vnc密码
    # vncserver
## 启动服务
    # systemctl daemon-reload
    # systemctl start vncserver@:3.service
    # systemctl enable vncserver@:3.service
## 测试连接
    # vncviewer 192.168.1.15:3
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjQ5MjUyMzc0LDIwNTg2NTI2MDJdfQ==
-->