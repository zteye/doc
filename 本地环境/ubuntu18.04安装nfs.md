# 1、NFS介绍

NFS 是 Network FileSystem 的缩写，顾名思义就是网络文件存储系统，它最早是由 Sun 公司发展出来的，也是 FreeBSD 支持的文件系统中的一个，它允许网络中的计算机之间通过 TCP/IP 网络共享资源。通过 NFS，我们本地 NFS 的客户端应用可以透明地读写位于服务端 NFS 服务器上的文件，就像访问本地文件一样方便。简单的理解，NFS 就是可以透过网络，让不同的主机、不同的操作系统可以共享存储的服务。

NFS可作为企业内部容器云的存储选项之一，实现容器的永久存储。

# 2、NFS 安装步骤

服务端 

```
apt install nfs-kernel-server
```



客户端 

 

```
apt install nfs-common
```

# 3、NFS 配置及使用
1）我们在服务端创建一个共享目录 /data/share ，作为客户端挂载的远端入口，然后设置权限。

 

```
 mkdir -p /data/share

 chmod 666 /data/share
```



 

2）然后，修改 NFS 配置文件 /etc/exports

 

```
 vim /etc/exports

/data/share 10.222.77.0/24(rw,sync,insecure,no_subtree_check,no_root_squash)
```



3）启动 RPC 服务

```
service rpcbind start
```



4）启动nfs服务

```
sudo service nfs-kernel-server restart
```



5）查看注册的端口列表

    rpcinfo -p localhostl
6）在服务端看下是否正确加载了设置的 /etc/exports 配置

```
showmount -e localhost
```



# 4、NFS测试

1) 在另一台 Linux 虚拟机上测试一下，是否能够正确挂载。可以在客户端查看下 NFS 服务端 (上边服务端 IP 为：192.168.232.105) 设置可共享的目录信息

showmount -e 192.168.232.105

2）在客户端创建挂载目录

mkdir -p /share

3) 挂载远程目录到客户端本地/share

mount 192.168.232.105:/data/share /share

手工在客户端的/share下创建一个a.txt，要以发现在服务器上的/data/share可以看见该文件。