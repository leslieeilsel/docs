# LCG - Linux Configuration Guide - Node https server
# node HTTPS服务器配置指南(Linux / Mac)

本项目为Vue+express

## openssl 生成证书文件


```
$ openssl version -a
```

![image](http://wx1.sinaimg.cn/mw690/656c44cbgy1fhu9dmz2fkj20s20kc760.jpg)

> 生成私钥key文件：

```
#生成私钥key文件
openssl genrsa -out privatekey.pem 1024
```
![image](http://wx1.sinaimg.cn/mw690/656c44cbgy1fhu9jrgby8j20wi0p4ju6.jpg)

> 通过私钥生成CSR证书签名


用该私钥生成一个证书，按照提示 输入国家缩写等信息

需要注意的是 common Name需要填写 认证的域名。

```
#通过私钥生成CSR证书签名
openssl req -new -key privatekey.pem -out certrequest.csr

---

You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:xxx
Locality Name (eg, city) []:xxx
Organization Name (eg, company) [Internet Widgits Pty Ltd]:xxx
Organizational Unit Name (eg, section) []:xxx
Common Name (eg, YOUR name) []:xxx
Email Address []:xxxx@gmail.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

> 对私钥和证书进行自签名的认证

通过私钥和证书签名生成证书文件


```
openssl x509 -req -in certrequest.csr -signkey privatekey.pem -out certificate.pem
```

## 用https模块起一个node服务。

到此为止我们需要的三个基础文件已经准备就绪，之后就是修改文件app.js

> 路径为：./production/app.js

~ vi production/app.js


```
//在最下面
var https = require('https')
var fs = require("fs");

var options = {
    key: fs.readFileSync('./production/privatekey.pem'),
    cert: fs.readFileSync('./production/certificate.pem'),
    ca: fs.readFileSync('./production/certrequest.csr')
};

https.createServer(options, app).listen(3011, function () {
    console.log('Https server listening on port ' + 3011);
});
```

## 打包并运行项目

> 注：解决http资源加载警告，
> 在打包项目前的index.html
> 加入

```
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```
> 打包并运行项目

```
npm run build

---
npm run start

---

Express server listening on port 3000
Https server listening on port 3011

```

这时我们起服务的时候Chrome浏览器中显示不安全提醒，这是因为浏览器没有ca认证的证书。
