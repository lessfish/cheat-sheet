# 1、文件确认

本地搭建 HTTPS 环境，首先确认下三个必需的文件，在 Apache 的 bin 目录下。如果以下过程中有相应文件的报错，可以替换试下。我部署过程就出现了错误，说 "openssl.exe 找不到序数 无法定位序数 296 于动态链接库 SSLEAY32.dll 上" 这样的错误，然后把三个文件全部替换，就 ok 了。

- bin/openssl.exe
- bin/libeay32.dll
- bin/ssleay32.dll

还有两个文件一并检查下：

- conf/openssl.cnf
- modules/mod_ssl.so

以 http://www.abc.com 为例，假设已经在 vhosts 中配置，可在本地访问  http://www.abc.com

# 2、为网站生成证书和私钥

打开 cmd，cd 进入 Apache 下面的 bin 目录，运行以下命令，生成私钥 **server.key**：

```
openssl genrsa 1024 > server.key
```

用上一步生成的的密钥生成证书请求文件 **server.csr**：

```
openssl req -new -key server.key > server.csr
```

这里会有一些参数需要填，以域名 www.abc.com  为例（最后两个可不输入，直接回车）：

```
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Shanghai
Locality Name (eg, city) []:Shanghai
Organization Name (eg, company) [Internet Widgits Pty Ltd]:abc
Organizational Unit Name (eg, section) []:abc
Common Name (eg, YOUR name) []:www.abc.com
Email Address []:abc@abc.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

# 3、通过 CA 为网站服务器签署证书

以下命令生成 CA 私钥 **ca.key**：

```
openssl genrsa -out ca.key 1024
```

利用 CA 的私钥生成 CA 的自签署证书 **ca.crt**：

```
 openssl req -new -x509 -days 365 -key ca.key -out ca.crt -config ..\conf\openssl.cnf
```

这里需要输入一系列参数，同上：

```
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Shanghai
Locality Name (eg, city) []:Shanghai
Organization Name (eg, company) [Internet Widgits Pty Ltd]:abc
Organizational Unit Name (eg, section) []:abc
Common Name (eg, YOUR name) []:www.abc.com
Email Address []:abc@abc.com
```

CA 为网站签署证书。最后在 bin 目录下创建 `demoCA`  文件夹，文件夹内包含 `index.txt`（空文件），`serial`（内容为 01，无后缀名），空文件夹 `newcerts`，然后执行命令，生成 **server.crt**：

```
openssl req -x509 -days 365 -key server.key -in server.csr > server.crt
```

至此，生成了如下五样东西：

- server.key
- server.csr
- ca.key
- ca.crt
- server.crt

可以打开来看看，如果是空文件，那就是出错了。


# 4、文件复制

将 server.crt 和 server.key 两个文件复制到 Apache 的 conf 文件夹下。


# 5、配置

打开 Apache 的 httpd.conf 文件，将如下两行注释去掉。第一行是在 Apache 中引入 ssl 模块，第二行启动 https 配置文件，如果需要切换 http/https，可以选择切换此行注释。

```
LoadModule ssl_module modules/mod_ssl.so
Include conf/extra/httpd-ssl.conf
```

打开  conf/extra/httpd-ssl.conf 文件，修改以下两行，使文件地址定位到操作 4 复制过去的正确地址，如果没打开这两行注释的话，打开。

```
SSLCertificateFile "C:/wamp/bin/apache/Apache2.2.21/conf/server.crt"
SSLCertificateKeyFile "C:/wamp/bin/apache/Apache2.2.21/conf/server.key"
```

然后再进行如下配置：

```
DocumentRoot "C:/wamp/www/www.abc.com"
ServerName www.abc.com
ServerAdmin abc@abc.com
```

关于 httpd-ssl.conf 文件，还有个坑是，该文件中有的地址是不存在的，需要手动修改指定到 Apache 中正确的位置，比如：

```
<Directory "C:/wamp/bin/apache/Apache2.2.21/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>
```

上面这几行原来的地址指向的是 "C:/apache2..."，我也不造为什么，反正我 C 盘里就没 apache2 这个文件夹，导致我排错了好久 ...


# 6、重启 Apache

重启 Apache，访问 https://www.abc.com

---

thinking：是否只需要 server.key, server.csr, server.crt 三个文件？

参考：

- <http://my.oschina.net/u/265943/blog/115401>
- <http://blog.360email.cn/apache/apache-https/>
- <http://openssl.6102.n7.nabble.com/error-while-generating-Certificate-Signing-Request-td16947.html>
- <http://www.flatmtn.com/article/setting-openssl-create-certificates>
- <http://stackoverflow.com/questions/7360602/openssl-and-error-in-reading-openssl-conf-file>


