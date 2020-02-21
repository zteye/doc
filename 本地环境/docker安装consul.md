```bash
docker run -d -p 8500:8500 -p 8300:8300 -p 8301:8301 -p 8302:8302 -p 8502:8502 -p 8600:8600  --restart=always -v /vagrant/consul/data/server1:/consul/data -v /vagrant/consul/conf/server1:/consul/config  --name=consul consul:1.6.3 agent -server -bootstrap-expect=1 -ui -client=0.0.0.0 -data-dir=/consul/data -config-dir=/consul/config
```

