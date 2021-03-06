# 1. VirtualBox本地网络构建

win10环境中，在“计算机”图标上右键选择“管理”，在打开的“计算机管理”窗口中选择左侧的“设备管理器”，然后在右侧图示的地方右键选择“添加过时硬件”。

 ![image-20200119152728384](images\image-20200119152728384.png)

最新版本的win10中位置有所改变，如下图:

 ![image-20200120124148790](images\image-20200120124148790.png)

在打开的窗口中点击“下一步”。


 ![image-20200119152813966](images\image-20200119152813966.png)



选择“安装我手动从列表中选择的硬件（高级）”，然后下一步。


 ![image-20200119152842999](images\image-20200119152842999.png)

把滚动条拉到最下面，选择“网络适配器”，然后下一步。


 ![image-20200119152956111](images\image-20200119152956111.png)

 在左侧的“厂商”中选择“Microsoft”，在右侧选择“Microsoft Loopback Adapter”，然后下一步，再下一步，开始安装驱动。

 ![image-20200120124400694](images\image-20200120124400694.png)

驱动安装完成后，会在网络适配器管理界面看到这张网卡。


 ![image-20200119153152485](images\image-20200119153152485.png)

接下来，就是设置物理无线网卡的网络共享。在无线网卡的属性对话框中，选择“共享”标签，按图中所示设置即完成了无线网卡的共享。


 ![image-20200119155145164](images\image-20200119155145164.png)

 现在设置虚拟机的网络，在如图所示的设置界面上，左侧选择“网络”，右侧按照图示方式设置。这样就完成了整个的设置操作，现在可以在虚拟机中ping通物理机了。 

![image-20200119155246288](images\image-20200119155246288.png)

# 2. 搭建vagrant环境

下载ubuntu box

16.04:

http://cloud-images.ubuntu.com/xenial/20200115/xenial-server-cloudimg-amd64-vagrant.box

18.04

http://cloud-images.ubuntu.com/bionic/20200116.1/bionic-server-cloudimg-amd64-vagrant.box

建议下载时登录ubuntu.com下载对应版本的最新package。



安装vagrant

一路下一步，此处省略



将box导入vagrant

```bash
vagrant box add ubuntu16.04 D:\vm\box\xenial-server-cloudimg-amd64-vagrant.box
```

