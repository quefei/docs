## CentOS 下创建/扩展逻辑卷

**1.1 获取相关信息：**

显示卷组 cl：

    vgdisplay -s cl

显示所有的逻辑卷：

    lvdisplay

显示所有文件系统的磁盘使用情况：

    df -h

**1.2 创建逻辑卷 backup：**








**1.3 扩展逻辑卷 usr_local：**

给逻辑卷 usr_local 增加 50G：

    lvextend -L +50G -f -r /dev/cl/usr_local

让逻辑卷 usr_local 达到 600G：

    lvextend -L 600G -f -r /dev/cl/usr_local

给逻辑卷 usr_local 增加 `剩余空间`：

    lvextend -l +100%FREE -f -r /dev/cl/usr_local
