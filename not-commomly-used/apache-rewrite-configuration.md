# Apache 中的 Rewrite 使用简介

## RewriteEngine

rewrite 的总开关，用来开启 url rewrite，想要打开，这样就可以了：

```
RewriteEngine on
```


## RewriteCond & RewriteRule

表示指令定义和匹配一个规则条件，让 RewriteRule 来重写。说的简单点，RewriteCond 就像我们程序中的 if 语句一样，表示如果符合某个或某几个条件则执行 RewriteCond 下面紧邻的 RewriteRule 语句，这就是 RewriteCond 最原始、基础的功能。

```
RewriteEngine on
RewriteCond  %{HTTP_USER_AGENT}  ^Mozilla//5/.0.*
RewriteRule  index.php   index.m.php
```

上面的匹配规则就是：如果匹配到 http 请求中 HTTP_USER_AGENT 是 Mozilla//5/.0.* 开头的，也就是用 FireFox 浏览器访问 index.php 这个文件的时候，会自动让你访问到 index.m.php 这个文件。

RewriteCond 和 RewriteRule 是上下对应的关系。可以有 1 个或者好几个 RewriteCond 来匹配一个 RewriteRule。


## HTTP_REFERER

这个匹配访问者的地址，php 中 $_REQUREST 中也有这个，当我们需要判断或者限制访问的来源的时候，就可以用它。

```
RewriteCond %{HTTP_REFERER} (www.test.cn)
RewriteRule (.*)$ test.php
```

上面语句的作用是如果你访问的上一个页面的主机地址是 www.test.cn，则无论你当前访问的是哪个页面，都会跳转到对 test.php 的访问。

再比如，也可以利用 HTTP_REFERER 防盗链，就是限制别人网站使用我网站的图片。

```
RewriteCond %{HTTP_REFERER} !^$ [NC]
RewriteCond %{HTTP_REFERER} !www.iyangyi.com [NC]
RewriteRule \.(jpg|gif) http://image.baidu.com/ [R,NC,L]
```

NC 是 nocase 的意思，忽略大小写。第一句呢，是必须要有域名，第二句就是看域名如果不是 www.iyangyi.com 的，当访问. jpg 或者. gif 文件时候，就都会自动跳转到 http://image.baidu.com/ 上，很好的达到了防盗链的要求。


## REQUEST_FILENAME

哪一块属于 REQUEST_FILENAME 呢？是 url 除了 host 域名外的。

```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^room/video/(\d+)\.html web/index\.php?c=room&a=video&r=$1 [QSA,NC,L]
```

- -d 是否是一个目录. 判断 TestString 是否不是一个目录可以这样: !-d
- -f 是否是一个文件. 判断 TestString 是否不是一个文件可以这样: !-f

这两句语句 RewriteCond 的意思是请求的文件或路径如果存在，返回已经存在的文件或路径，如果不存在，则执行 RewriteRule  的重定向。

上面 RewriteRule 正则的意思是以 room 开头的 room/video/123.html 这样子，变成 web/index.php?c=room&a=video&r=123（如果文件或路径不存在的话）


## RewriteRule 写法和规则

RewriteRule 是配合 RewriteCond 一起使用，可以说，RewriteRule 是 RewriteCond 成功匹配后的执行结果。

```
RewriteRule Pattern Substitution [flags]
```
Pattern 是一个正则匹配，Substitution 是匹配的替换， [flags] 是一些参数限制。

```
RewriteRule ^room/video/(\d+)\.html web/index\.php?c=room&a=video&r=$1 [QSA,NC,L]
```

意思是 以 room 开头的 room/video/123.html 这样子，变成 web/index.php?c=room&a=video&r=123

```
RewriteRule \.(jpg|gif) http://image.baidu.com/ [R,NC,L]
```

意思是以为是访问. jpg 或者 gif 的文件，都会跳转到 http://image.baidu.com

我们再看看 [flags] 是什么意思？

- R redirect(强制重定向) 的意思，适合匹配 Pattern 后，Substitution 是一个 http 地址 url 的情况，就跳转出去了。上面那个跳转到 image.baidu.com 的例子，就必须也用它。
- L last(结尾规则) 的意思，就是已经匹配到了，就立即停止，不再匹配下面的 Rule 了，类似于编程语言中的 break 语法，跳出去了。

sample:

```
<VirtualHost *:80>
    ServerName www.rewrite.com
    DocumentRoot "C:/wamp/www/www.rewrite.com"
    <Directory "C:/wamp/www/www.rewrite.com">
        RewriteEngine on
        RewriteRule /admin/api/oalogin.php(.*)$ /admin/login/oalogin$1 [R=301,L]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule ^(.*)$ index.php/$1 [L]
    </Directory>
</VirtualHost>
```

## Read more

- [apache 的虚拟域名 rewrite 配置以及. htaccess 的使用](https://www.zybuluo.com/phper/note/73726)
- [Apache Rewrite url 重定向功能的简单配置](http://www.jb51.net/article/24435.htm)
- [Apache Rewrite 规则详解](http://www.ha97.com/3460.html)
- [Apache 模块 mod_rewrite](http://man.chinaunix.net/newsoft/ApacheMenual_CN_2.2new/mod/mod_rewrite.html)
- [Apache mod_rewrite](http://httpd.apache.org/docs/2.4/zh-cn/rewrite/)

