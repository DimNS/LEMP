# Nginx

### Подключение официального репозитория
```
apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | tee /usr/share/keyrings/nginx-archive-keyring.gpg > /dev/null
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/debian `lsb_release -cs` nginx" | tee /etc/apt/sources.list.d/nginx.list
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" | tee /etc/apt/preferences.d/99nginx
```

### Установка
```
apt update
apt policy nginx
apt install nginx -y
service nginx status
mkdir /var/www
chown -R www-data:www-data /var/www
passwd www-data
grep www-data /etc/passwd
nano /etc/passwd
```
> Заменить `/usr/sbin/nologin` на `/bin/bash`

\* Этим мы включаем возможность входить по SSH пользователю `www-data`

### Первичная настройка
```
nano /etc/nginx/nginx.conf
```
> Содержимое для файла:
> ```
> user  www-data;
> worker_processes  auto;
> 
> error_log  /var/log/nginx/error.log notice;
> pid        /var/run/nginx.pid;
> 
> events {
>     worker_connections  1024;
> }
> 
> http {
>     charset UTF-8;
>     sendfile on;
>     tcp_nopush on;
>     tcp_nodelay on;
>     keepalive_timeout 65;
>     types_hash_max_size 2048;
>     server_tokens off;
> 
>     include       /etc/nginx/mime.types;
>     default_type  application/octet-stream;
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
> 
>     ##
>     # Virtual Host Configs
>     ##
> 
>     include /etc/nginx/conf.d/*.conf;
>     include /etc/nginx/sites-enabled/*;
> }
> ```
Проверяем и перечитываем конфиг:
```
nginx -t
service nginx reload
```
