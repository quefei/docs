## CentOS 下手动安装网卡驱动

**1.1 安装 pciutils-3.5.1-2.el7.x86_64.rpm：**

下载 [pciutils-3.5.1-2.el7.x86_64.rpm][1] 到 CentOS 中，然后安装：`rpm -ivh pciutils-3.5.1-2.el7.x86_64.rpm`

**1.2 获取设备代码：**

执行命令：`lspci -nn | grep -i net`，获得设备代码，形如：`[10de:03ef]`

**1.3 获取与设备代码相应的 rpm 包：**

打开页面 [http://elrepo.org/tiki/DeviceIDs][2]，搜索代码 `10de:03ef`，获得 rpm 包的信息：`pci    10DE:03EF    kmod-forcedeth`
















  [1]: http://mirrors.aliyun.com/centos/7/os/x86_64/Packages/pciutils-3.5.1-2.el7.x86_64.rpm
  [2]: http://elrepo.org/tiki/DeviceIDs