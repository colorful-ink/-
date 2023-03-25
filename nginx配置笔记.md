```java
http{
    server{
        location / {
        
        }
    }
}
```
在配置文件中，内层块未定义的值会自动获取外层块的值作为默认值（有例外，详见参考）

#### 进程相关`worker_...`
>worker_processes 8 #设置进程数为8，(可设为auto)
>worker_rlimit_nofile
>worker_connections
### 服务器`server`
```conf
server {
    listen 80;  #监听端口
    sever_name www.colorfullinks.ink;   #域名信息，为虚拟服务器的识别路径
    
    location / {    #location 后可跟 ~ 加正则表达式，检测域名后的地址
        root /www/www;  #网站根目录绝对地址（可写在server中）
        index index.html index.htm; #默认首页类型（可写在server中）
        deny 192.168.2.11;  #禁止访问的ip地址，可为all
        allow 192.168.2.11; #允许访问的ip地址，可为all
    }
}
```