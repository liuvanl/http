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
