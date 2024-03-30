---
layout: post
title: "Linux下创建浏览器信任的SSL证书"
categories: linux ssl
tags: ssl https apache
---
## 1 创建自签名证书
* 建立服务器密钥
```
openssl genrsa -des3 1024  > server.key 
```
* 从密钥中删除密码（以避免系统启动后被询问口令） 
```
openssl rsa -in server.key > server2.key
mv server2.key server.key
```
* 建立服务器密钥请求文件
注意COMMON_NAME要和后续步骤DNS.1保持一致，避免部分软件的校验。
```
openssl req -new -key server.key -out server.csr
```
* 建立服务器证书  
首先修改openssl.cnf, 创建ext.ini, 再使用如下命令创建服务器证书server.cert
```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.cert -extfile ext.ini
```

openssl.cnf(默认路径/etc/ssl/openssl.cnf)修改内容：

```
# 确保req下存在以下2行（默认第一行是有的，第2行被注释了）
[ req ]
distinguished_name = req_distinguished_name
req_extensions = v3_req

# 确保req_distinguished_name下没有 0.xxx 的标签，有的话把0.xxx的0. 去掉
[ req_distinguished_name ]
countryName              = Country Name (2 letter code)
countryName_default = CN
stateOrProvinceName             = State or Province Name (full name)
stateOrProvinceName_default = ShangHai
localityName              = Locality Name (eg, city)
localityName_default = ShangHai
organizationalUnitName             = Organizational Unit Name (eg, section)
organizationalUnitName_default = Domain Control Validated
commonName         = Internet Widgits Ltd
commonName_max = 64

# 新增最后一行内容 subjectAltName = @alt_names（前2行默认存在）
[ v3_req ]
# Extensions to add to a certificate request
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

# 新增 alt_names,注意括号前后的空格，DNS.x 的数量可以自己加
[ alt_names ]
DNS.1 = abc.example.com
```   
ext.ini(自定义路径)文件内容:
```
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = abc.example.com
```

## 2 Apache2 关联ssl配置，启动ssl
* 创建ssl配置文件软连接，关联文件
```
    ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/001-ssl.conf
```

* 关联ssl文件  
修改default-ssl.conf中的下面两行：
```
    SSLCertificateFile 证书路径.crt
    SSLCertificateKeyFile 私钥路径.key
```
* 加载模块重启服务
```
    sudo a2enmod ssl   #加载模块
    sudo service apache2 restart # 重启服务
```

## 3 windows端保存证书
点击地址栏感叹号按钮，导出证书再安装证书即可  
![入口]({{  site.url }}/img/2021-08-15-how-to-config-self-sign-SSL-cert/export_cert.png)  
![保存证书]({{  site.url }}/img/2021-08-15-how-to-config-self-sign-SSL-cert/save_cert.png)  
# 参考
https://www.dyxmq.cn/network/create-self-sign-ca-and-certificate.html
https://blog.csdn.net/weixin_33804582/article/details/91600006
https://blog.csdn.net/u010820857/article/details/89226799
