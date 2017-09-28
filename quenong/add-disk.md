## CentOS 下增加一块磁盘

**1.1 磁盘说明：**

增加的磁盘不应该存在 CentOS，如果存在 CentOS，应该将其彻底删除，例如：

 - 运行 Windows PE
 - 使用 DG 分区工具
 - 选择要操作的磁盘
 - 通过快速分区
 - 选择 MBR 等选项
 - 完成后重新开机
 - CentOS 已识别磁盘

**1.2 列出磁盘：**

    parted -l | grep -C 10 "scsi"

**1.3 以交互模式操作：**

    parted /dev/sdb

**1.4 给磁盘分区：**

    [root@quenong ~]# parted /dev/sdb
    
    GNU Parted 3.1
    Using /dev/sdb
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) mklabel gpt ......................................... # 1 - 定义分区表格式
    Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be lost. 
    Do you want to continue?
    Yes/No? yes .................................................. # 2 - 确认操作
    (parted) mkpart .............................................. # 3 - 创建分区
    Partition name?  []?
    File system type?  [ext2]?
    Start? 1 ..................................................... # 4 - 定义分区的起始位置
    End? -1 ...................................................... # 5 - 定义分区的结束位置，-1 代表到末尾
    (parted) print ............................................... # 6 - 查看分区
    Model: ATA WDC WD10EZEX-08W (scsi)
    Disk /dev/sdb: 1000GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: gpt
    Disk Flags:
    
    Number  Start   End     Size    File system  Name  Flags
     1      1049kB  1000GB  1000GB  ntfs
    
    (parted) toggle 1 lvm ........................................ # 7 - 给 1 号分区标记为 lvm
    (parted) print ............................................... # 8 - 查看分区
    Model: ATA WDC WD10EZEX-08W (scsi)
    Disk /dev/sdb: 1000GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: gpt
    Disk Flags:
    
    Number  Start   End     Size    File system  Name  Flags
     1      1049kB  1000GB  1000GB  ntfs               lvm
    
    (parted) quit ................................................ # 9 - 退出
    Information: You may need to update /etc/fstab.

**1.5 重新读取分区信息：**

    partprobe /dev/sdb

**1.6 将分区 /dev/sdb1 创建为物理卷：**

    pvcreate /dev/sdb1

**1.7 显示所有的物理卷：**

    pvdisplay

**1.8 创建卷组 el，并且将物理卷 /dev/sdb1 加入卷组：**

    vgcreate el /dev/sdb1

**1.9 显示卷组 el：** 

    vgdisplay el

**1.10 在卷组 el 上，创建逻辑卷 backup，分配所有的空闲空间：**

    lvcreate -l +100%FREE -n backup el

**1.11 显示卷组 el 的逻辑卷：**

    lvdisplay el

**1.12 格式化逻辑卷 backup：**

    mkfs.xfs -f /dev/el/backup

**1.3 手动挂载逻辑卷 backup：**

    mkdir -p /backup
    mount /dev/el/backup /backup
    df -h

