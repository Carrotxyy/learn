## Vue router history模式下Nginx的配置

#### 单一项目配置

只有单一项目需要部署，则root直接指向目标文件夹，即包含index.html的文件夹路径

```nginx
server {
    listen 80;
    server_name manager.xxx.com;
    
    location / {
        root /nginx/manager/dist/;
        try_files $uri $uri/ /index.html;
        index index.html;
    }
}
```



#### 多项目配置

此种配置适合同一域名下的多个项目

```nginx
server {
    listen 80;
    server_name manager.xxx.com;
    
    location /project1 {
        try_files $uri $uri/ /index.html;
        root /nginx/manager/project1/dist;
        index index.html
    }
    
    location /project2 {
        try_files $uri $uri/ /index.html;
        root /nginx/manager/project2/dist;
        index index.html
    }
}
```



#### try_files

```nginx
try_files $uri $uri/ /index.html;
```

当用户请求 http://manager.xxx.com/test时，

**&uri** 为 /test

try_files 会在硬盘中尝试找这个文件: `/$root/test`

如果存在，直接发送文件内容给用户，如不存在则继续下一个变量

如果前面的变量都找不到，则会 **fall back** 到 try_files的最后一个选项 /index.html

相当于 nginx 发起一个`HTTP`请求  http://localhost/$root/index.html