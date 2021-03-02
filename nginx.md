# Nginx

### Установка
```
add-apt-repository ppa:nginx/stable
apt update
apt policy nginx
apt install nginx -y
service nginx status
chown -R www-data:www-data /var/www
passwd www-data
grep www-data /etc/passwd
nano /etc/passwd
```
> Заменить `/usr/sbin/nologin` на `/bin/bash`

### Первичная настройка
```
nano /etc/nginx/nginx.conf
```
> Содержимое для файла:
> ```
> http {
>     charset UTF-8;
>     sendfile on;
>     tcp_nopush on;
>     tcp_nodelay on;
>     keepalive_timeout 65;
>     types_hash_max_size 2048;
>     server_tokens off;
> 
>     ##
>     # SSL Settings
>     ##
> 
>     ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
>     ssl_prefer_server_ciphers on;
>     ssl_session_cache shared:SSL:10m;
>     ssl_session_timeout 10m;
> 
>     ##
>     # Logging Settings
>     ##
>     
>     log_format main '$host: $remote_addr [$time_local] '
>         '"$request" "$http_referer" $status $bytes_sent '
>         '"$http_user_agent" "$http_x_forwarded_for" rt=$request_time';
> 
>     access_log /var/www/nginx_access.log main;
>     error_log /var/www/nginx_error.log;
> 
>     ##
>     # Gzip Settings
>     ##
>     
>     gzip on;
>     gzip_min_length 1000;
>     gzip_buffers 16 8k;
>     gzip_types text/plain text/css text/xml application/x-javascript application/xml application/xhtml+xml;
> }
> ```
Проверяем и перечитываем конфиг:
```
nginx -t
service nginx reload
```
