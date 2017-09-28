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
    (parted) mklabel gpt .........................................
    Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be lost. Do you want to continue?
    Yes/No? yes ..................................................
    (parted) mkpart ..............................................
    Partition name?  []?
    File system type?  [ext2]?
    Start? 1 .....................................................
    End? -1 ......................................................
    (parted) print ...............................................
    Model: ATA WDC WD10EZEX-08W (scsi)
    Disk /dev/sdb: 1000GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: gpt
    Disk Flags:
    
    Number  Start   End     Size    File system  Name  Flags
     1      1049kB  1000GB  1000GB  ntfs
    
    (parted) toggle 1 lvm ........................................
    (parted) print ...............................................
    Model: ATA WDC WD10EZEX-08W (scsi)
    Disk /dev/sdb: 1000GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: gpt
    Disk Flags:
    
    Number  Start   End     Size    File system  Name  Flags
     1      1049kB  1000GB  1000GB  ntfs               lvm
    
    (parted) quit ................................................
    Information: You may need to update /etc/fstab.











