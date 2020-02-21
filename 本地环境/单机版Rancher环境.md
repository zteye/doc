启动rancher：

```bash
docker run -d --restart=unless-stopped \
-p 8080:80 -p 8443:443 \
-v /home/vagrant/rancher:/var/lib/rancher/ \
-v /root/var/log/auditlog:/var/log/auditlog \
-e CATTLE_SYSTEM_CATALOG=bundled \
-e AUDIT_LEVEL=3 \
rancher/rancher:stable
```

登录rancher，使用一台新机器node1配置etcd、controller，也可以使用rancher所在机器。

```bash
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.3.5 --server https://192.168.137.101:8443 --token 4r8f2qp92wswhrq2lpdx2gl8cgsrbrrtftglzm56whddhwtzj4wm2j --ca-checksum 9a6d8e7ed629250043d3e6b396cd3949ee32b163589919b39468b4ee1fc33503 --etcd --controlplane
```

再使用一台新机器node2，为集群添加worker

```bash
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.3.5 --server https://192.168.137.101:8443 --token 4r8f2qp92wswhrq2lpdx2gl8cgsrbrrtftglzm56whddhwtzj4wm2j --ca-checksum 9a6d8e7ed629250043d3e6b396cd3949ee32b163589919b39468b4ee1fc33503 --worker
```

为了测试方便，再添加一台work节点node3

```bash
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.3.5 --server https://192.168.137.101:8443 --token 4r8f2qp92wswhrq2lpdx2gl8cgsrbrrtftglzm56whddhwtzj4wm2j --ca-checksum 9a6d8e7ed629250043d3e6b396cd3949ee32b163589919b39468b4ee1fc33503 --worker
```

等待一会，10分钟左右把，集群创建成功，登录rancher查看：

![image-20200221111756015](images\image-20200221111756015.png)