# 第一章、http基础
## 1.1 配置网站/配置虚拟主机
### 看一个实际需求
> 我们在实际访问网站的过程中，不可能使```http://localhost:80```方式去访问网站，实际上使用类似:```http://www.sina.com.cn```或者```http://www.blog.com```方式去访问网站，这个又是怎么实现的呐？
### 步骤
1. 打开apache/conf/httpd.conf文件
```
  # Virtual hosts
  Include conf/extra/httpd-vhosts.conf
```
2. 找到host文件c:/windows/system32/drivers/etc/hosts
```
  127.0.0.1 www.liuvanl.com
```
3. 正式的配置虚拟主机文件httpd-vhosts.conf
> 该文件在apache/conf/extra/httpd-vhosts.conf 
```
  <VirtualHost *:80>
    # DocumentRoot 就是该网站的根目录
    DocumentRoot "E:\Program Files (x86)\wamp\www\liuvanl"
    # 主机名，域名
    ServerName www.liuvanl.com
  </VirtualHost>
```
4. 重启appache
5. 我们访问时，就可以通过域名访问了
## 1.2 配置虚拟主机的细节说明
1. 如何查看错误日志   到appache/log/error.log查看即可
2. 如何配置默认的网站首页
```
  <VirtualHost *:80>
    DocumentRoot "E:\Program Files (x86)\wamp\www\liuvanl"
    ServerName www.liuvanl.com
    # 配置网站默认首页面
    DirectoryIndex index.html
  </VirtualHost>
```
3. 如何配置访问某个网站的权限
```
  <VirtualHost *:80>
    # DocumentRoot 就是该网站的根目录
    DocumentRoot "E:\Program Files (x86)\wamp\www\liuvanl"
    # 主机名，域名
    ServerName www.liuvanl.com
    DirectoryIndex index.html
    # 配置访问权限
    <Directory "E:\Program Files (x86)\wamp\www\liuvanl">
    # 所有人都可以访问
    # Require all granted
    # 所有人不可以访问
    # Require all denied
    # 指定可以访问的ip
    require local
    <Directory>
  </VirtualHost>
```
4. 当我们配置了虚拟主机后，虚拟主机的管理都在httpd-vhost.conf 文件，你把localhost当做一个主机配置即可
# 第二章、http协议请求
## 2.1 http协议的基本概念
> 超文本传输协议(HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。是工作在tcp/ip协议基础上的,所有的WWW文件都必须遵守这个标准。设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法
### 说明
1. http协议也叫超文本传输协议
2. http协议用于传输文本和图片（等文件）
3. 在建立tcp/ip协议基础至上的
## 2.2 http请求-基本介绍
1. Request URL: `http://www.liuvanl.com/` 请求的地址
2. Request Method: GET  请求的方式
3. Status Code: 304 Not Modified   http状态码
## 2.3 http请求-请求行的操作
### 请求方式说明
> POST GET HEAD OPTIONS DELETE TRACE PUT  
> 在我们实际的开发中，使用到的请求的方式只有两种 get, post
### get和post区别
1. get方式提交的数据会放在url后面，而post提交方式不会，post提交的数据会放在http请求的实体内容部分
2. post的安全性比get方式要好.
3. 两种方式提交的数据长度大小
> 传输数据的大小：http协议没有对传输的数据大小进行限制，http协议规范也没有对URL长度进行限制。而在实际开发中存在限制主要有：
+ GET:特定浏览器和服务器对URL长度有限制，例如IE对URL长度的限制是2083字节（2k+35）。对于其它浏览器，理论上没有限制，其限制取决于操作系统的支持
+ POST：由于不是通过URL传值，理论上数据不受限
#### 说明
+ get的大小是有浏览器和操作系统来限时
+ post数据理论上没有显示.
+ 如果我们要上传或者下载文件，可以使用http协议，但是该文件一般不要大于2m, 如果将来我们需要上传或者下载很大文件，可以这样1. 写插件 2. 开发专门的客户端来做.
4. 安全性: 相对而言post提交，安全高
5. get提交 更利于添加到我的收藏夹
6. 默认情况下，http是get请求当我们一个表单，没有写method属性值，默认是get方式提交
7. 如果是小数量数据，并不要求安全性，则选择get,否则post
8. 如果是一个 超链接带参数，也是get方式提交的
## 2.4 http请求-消息头
1. Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8 浏览器可以接受的数据
2. Accept-Encoding: gzip, deflate  浏览器可以接受的压缩格式
3. Accept-Language: zh-CN,zh;q=0.9  可以接收的语言
4. Host: `www.liuvanl.com`    请求的主机名
5. If-Modified-Since: Tue, 31 Mar 2020 07:59:45 GMT  浏览本地已经有这个资源，这个资源的最新更新的时间
6. referer:  告诉浏览器，这个请求是从哪个页面来的
7. User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36  浏览器内核
## 2.5 用于http请求中的常用头 应用案例
### 案例1
> 如何获取所有的http请求的消息头（拒绝对192.168服务）
```php
  <?php
    header('content-type:text/html;charset=utf8');
    // 如何获取所有的http请求的消息头（拒绝对192.168服务）
    // var_dump($_SERVER)		// 输出所有的消息头
    $remote_addr = $_SERVER['SERVER_ADDR'];

    /**
      param strpos($remote_addr,'192.168')，该函数可以判断$remote_addr中是否含有192.168，如果有就返回一个值，表示在$remote_addr串中第几个位置，返回0表示，开头就出现
    */
    if($remote_addr == '' || strpos($remote_addr,'192.168') === 0){
      echo '<br>你的地址有问题';
    }else {
      echo '<br>通过验证';
    }
  ?>
```
