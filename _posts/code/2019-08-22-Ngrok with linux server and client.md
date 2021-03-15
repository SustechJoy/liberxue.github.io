---
layout: blog
comments: true
code: true
title:  "Config Ngrok Server to implement Intranet penetration"
tags:
- Ngrok
- network
background-image: https://mannyisles.com/wp-content/uploads/2018/04/ngrok.png
date:   2019-08-22 18:50:00
category: code
---

## Ngrok with linux server and client

应用场景：

```
1. 不满足于云服务器硬盘空间，需要配置NAS服务。
2. 安全性场景。
3. 远程办公，内网主机ssh。
```

准备：

```
公网主机
域名及解析服务
内网主机
```

操作步骤：

1. 配置域名解析

![](https://i.loli.net/2021/03/15/dFipaDuU6nZlq1w.png)

此网络应用需打开多个端口，如云服务器存在端口防火墙，请逐一开启。

1. 安装golang环境

golang1.5以后实现了自编译,也就是用golang开发golang。因此在安装新版本的golang时需要先安装一个golang1.4版本

安装golang1.4

```
wget https://storage.googleapis.com/golang/go1.4-bootstrap-20170531.tar.gz
tar -xf go1.4-bootstrap-20170531.tar.gz
cd go/src
./make.bash
```

成功后信息

```
Installed Go for linux/amd64 in /home/test/go
Installed commands in /home/test/go/bin
```

回到安装根目录

```
mv go go1.4
```

安装golang1.9

```
wget https://storage.googleapis.com/golang/go1.9.src.tar.gz
tar -xf go1.9.src.tar.gz
cd go/src
./all.bash
```

回到安装根目录

```
mv go /usr/local/go1.9
vim /etc/profile
export PATH=$PATH:/usr/local/go1.9/bin
source /etc/profile
```

测试安装成功

```
go version
```

成功返回

```
go version go1.9.2 linux/amd64
```

3. 安装ngrok

下例中的weixin.yangjiace.xyz为源域名的二级域名。具体配置方式参考步骤1

```
git clone https://github.com/tutumcloud/ngrok.git ngrok
cd ngrok

NGROK_DOMAIN="weixin.yangjiace.xyz"

openssl genrsa -out base.key 2048

openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem

openssl genrsa -out server.key 2048

openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr

openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt
```

4. 替换证书

```
cp base.pem assets/client/tls/ngrokroot.crt
```

5. 编译

```
make release-server release-client
```

编译成功后会在bin目录下找到ngrokd和ngrok这两个文件，其中ngrokd 就是服务端程序了

6. 启动服务器端

```
./bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="weixin.yangjiace.xyz" -httpAddr=":80" -httpsAddr=":443"
```

7. 编译客户端

```
#根据操作系统选择
GOOS=windows GOARCH=amd64 make release-client  
GOOS=darwin GOARCH=amd64 make release-client  
GOOS=linux GOARCH=amd64 make release-client  
```

编译结束后在bin目录下生成对应的ngrok文件，拷贝到本地，linux可使用scp

```
scp -P server_ssh_port name@server_address/target_folder/target_file
```

8. 设置服务器端

```
./bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="weixin.yangjiace.xyz" -httpAddr=":80" -httpsAddr=":443"
```

9. 设置本地客户端

在ngrok的同级目录下新建ngrok.yml文件，写入具体配置信息

```
cd ~
vim ngrok.yml
```

如下配置设置了讲内网主机的22端口映射至目标外网主机51200端口，同时将内网主机80端口应用映射到外网主机域名test.[hostname]下

```
server_addr: "cdgngrok.ayyxxx.com:4443"
tunnels:
    ssh:
        remote_port: 51200 #远程端口
        proto:
            tcp: ":22"
    web:
        subdomain: test  #子域名
        proto:
            http: ":80"
```

可以参考<https://www.bejson.com/validators/yaml/>来检测yml文件是否格式有误，

启动服务

```
./ngrok -config=ngrok.yml start ssh web
```

启动成功

![](https://i.loli.net/2021/03/15/VK6jyNzYgZtpiqI.png)

10. 可以在内网主机localhost:4000检查连接状态

![](https://i.loli.net/2021/03/15/iHLYDAb9F8s3ZcX.png)

Reference

[1]. <https://blog.csdn.net/u011019726/article/details/77584708>

[2]. <https://blog.csdn.net/qq_38795209/article/details/78141132>

[3]. <https://blog.csdn.net/yjc_1111/article/details/79353718>

[4]. <https://ngrok.com/>

