ubuntu18.04 安装keepalived



在Ubuntu18.04 LTS 安装软件

```
sudo apt install keepalived
```

刚安装完，配置文件目录/etc/keepalived/是空的，把下面的内容保存为文件keepalived.conf放到该目录下。

- 注意：
  - 虚拟IP会占用一个物理网卡地址。通过配置文件指定。
  - 使用 ifconfig查看网卡的de vice接口信息，选择其它服务不用的网卡用于虚拟IP。
  - 因为keepalived会动态改变绑定网卡的IP地址，不要把其它服务绑定到该网卡端口和IP地址上。

## 配置文件

master配置文件/etc/keepalived/keepalived.conf：

```
! Configuration File for keepalived

global_defs {

}


vrrp_instance VI_1 {

    # 初始化状态
    state MASTER

    # 虚拟 ip 绑定的网卡
    interface enp0s3 

    # 此 ID 要与 Backup 配置一致
    virtual_router_id 80

    # 默认启动优先级，要比 Backup 大点，但要控制量，保证自身状态监测生效
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }

    virtual_ipaddress {
        # 虚拟 ip 地址
        192.168.137.111 
    }

}
```

重启master keepalived

```
service  keepalived start
```



BACKUP配置文件：

```
! Configuration File for keepalived

global_defs {

}


vrrp_instance VI_1 {

    # 初始化状态
    state BACKUP

    # 虚拟 ip 绑定的网卡
    interface enp0s3 

    # 此 ID 要与 Backup 配置一致
    virtual_router_id 80

    # 默认启动优先级，要比 Backup 大点，但要控制量，保证自身状态监测生效
    priority 90
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }

    virtual_ipaddress {
        # 虚拟 ip 地址
        192.168.137.111 
    }

}
```

重启backup keepalived

```
service  keepalived start
```



查看master的ip地址

![image-20200223165615818](images\image-20200223165615818.png)

停止master keepalived

```
service  keepalived stop
```

发现IP漂移到backup上。