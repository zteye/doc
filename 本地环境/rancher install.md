./create_self-signed-cert.sh --ssl-domain=www.rancher.zteye --ssl-trusted-domain=www.rancher.zteye \
--ssl-trusted-ip=192.168.137.101,192.168.137.102,192.168.137.103,192.168.137.110,192.168.137.200 --ssl-size=2048 --ssl-date=3650


kubectl  -n kube-system \
    create serviceaccount tiller
kubectl create clusterrolebinding tiller \
    --clusterrole cluster-admin \
    --serviceaccount=kube-system:tiller





    helm init -i zteye.harbor:80/zteye/gcr.io.kubernetes-helm.tiller:v2.12.3 --stable-repo-url http://mirror.azure.cn/kubernetes/charts/ --service-account tiller --override spec.selector.matchLabels.'name'='tiller',spec.selector.matchLabels.'app'='helm' --output yaml | sed 's@apiVersion: extensions/v1beta1@apiVersion: apps/v1@' | kubectl apply -f -



docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /etc/nginx.conf:/etc/nginx/nginx.conf \
  -v /opt/ca/:/opt/ca/ \
  --add-host www.rancher.local:192.168.137.110 \
  nginx:1.14



    helm install ./rancher \
    --name rancher \
    --namespace cattle-system \
    --set hostname=www.rancher.zteye \
    --set ingress.tls.source=secret \
    --set privateCA=true \
    --set useBundledSystemChart=true \
    --set rancherImage=zteye.harbor:80/rancher/rancher

kubectl  create namespace cattle-system

kubectl -n cattle-system create \
    secret tls tls-rancher-ingress \
    --cert=./tls.crt \
    --key=./tls.key

    kubectl -n cattle-system \
    create secret generic tls-ca \
    --from-file=cacerts.pem





常用命令
helm delete --purge rancher



docker参数

```json
{
  "registry-mirrors": ["https://8vd4zs15.mirror.aliyuncs.com"],
  "max-concurrent-downloads": 10,
  "max-concurrent-uploads": 5,
  "insecure-registries":["zteye.harbor:80"]
}
```



# 搭建consul集群

三台主机上建立数据目录和配置目录，目录为空即可：

```bash
mkdir -p /app/consul/data

mkdir -p /app/consul/conf
```

三台主机依次启动容器：

```bash
docker run --net=host --name consul -v /app/consul/data:/consul/data -v /app/consul/conf:/consul/config -d zteye.harbor:80/zteye/consul consul agent -server -bind=192.168.137.110 -client 0.0.0.0 -ui -bootstrap-expect=3 -data-dir /consul/data -config-dir /consul/config

docker run --net=host --name consul -v /app/consul/data:/consul/data -v /app/consul/conf:/consul/config -d zteye.harbor:80/zteye/consul consul agent -server -bind=192.168.137.101 -client 0.0.0.0 -ui -bootstrap-expect=3 -data-dir /consul/data -config-dir /consul/config  -join  192.168.137.110

docker run --net=host --name consul -v /app/consul/data:/consul/data -v /app/consul/conf:/consul/config -d zteye.harbor:80/zteye/consul consul agent -server -bind=192.168.137.102 -client 0.0.0.0 -ui -bootstrap-expect=3 -data-dir /consul/data -config-dir /consul/config  -join  192.168.137.110
```

