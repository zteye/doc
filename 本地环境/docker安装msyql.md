先下载mysql，默认下载的latest版本。

```bash
docker pull mysql
```



启动mysql

```bash
docker run -p 3306:3306 --name mysql \
-v /home/vagrant/mysql/conf:/etc/mysql \
-v /home/vagrant/mysql/logs:/var/log/mysql \
-v /home/vagrant/mysql/data:/var/lib/mysql \
-v /home/vagrant/mysql-files:/var/lib/mysql-files \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql
```

登录

```bash
docker exec -it mysql bash
mysql -uroot -p123456
```

个别客户端连接时会出错

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
```

