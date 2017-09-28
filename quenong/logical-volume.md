## CentOS 下创建/扩展逻辑卷

**1.1 获取相关信息：**

显示卷组 cl：

    vgdisplay -s cl

显示所有的逻辑卷：

    lvdisplay

显示所有文件系统的磁盘使用情况：

    df -h

**1.2 创建逻辑卷 backup：**

在卷组 `cl` 上，创建逻辑卷 backup，分配 200G 空间：

    lvcreate -L 200G -n backup cl

或者分配 `剩余空间`：

    lvcreate -l +100%FREE -n backup cl

获得设备 `/dev/cl/backup`，用于挂载。

然后：

 - 格式化：`mkfs.xfs -f /dev/cl/backup`
 - 挂载点：`mkdir -p /backup`
 - 手动挂载：`mount /dev/cl/backup /backup`
 - 使用情况：`df -h`

**1.3 扩展逻辑卷 usr_local：**

给逻辑卷 usr_local 增加 50G：

    lvextend -L +50G -f -r /dev/cl/usr_local

让逻辑卷 usr_local 达到 600G：

    lvextend -L 600G -f -r /dev/cl/usr_local

给逻辑卷 usr_local 增加 `剩余空间`：

    lvextend -l +100%FREE -f -r /dev/cl/usr_local
