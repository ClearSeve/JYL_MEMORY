# nginx

## conf

```

=======server
http {}中可以包含多个server

server {
    listen        80;
    server_name   localhost;

    location / {
        root          D:/dist;
        index         index.html index.htm;
    }
}

=======转发

location / {
    proxy_pass 1.2.3.4:80;
}
访问host时，就都被转发到1.2.3.4:80。


=======负载均衡
upstream myserver; {
    ip_hash;
    server 172.16.1.1:8001;
    server 172.16.1.2:8002;
    server 172.16.1.3;
    server 172.16.1.4;
}
location / {
    proxy_pass http://myserver;
}
在 upstream 中指定了一组机器，并将这个组命名为 myserver，这样在 proxypass 中只要将请求转移到 myserver 这个 upstream 中我们就实现了在四台机器的反向代理加负载均衡。ip_hash 指明了我们均衡的方式是按照用户的 ip 地址进行分配
```

## windows

配置文件nginx.conf

重启服务：  
nginx -s reload

## centeros

```
wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum install nginx

systemctl start  nginx  启动nginx服务
systemctl stop   nginx
systemctl reload nginx
systemctl status nginx


查看nginx所在位置
whereis nginx


默认的配置文件在 /etc/nginx/conf.d/default.conf
修改/etc/nginx/nginx.conf中启动用户名：  user  root;
```
