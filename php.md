# PHP
> Обратите внимание на использование буквы `x` в нескольких местах: `7.x`, `status7x`
> Их необходимо заменить на номер необходимой вам версии PHP

### Установка нескольких версий PHP
```
add-apt-repository ppa:ondrej/php
apt update
apt show php
apt install php7.2 php7.2-fpm php7.2-curl php7.2-gd php7.2-mysql php7.2-mbstring php7.2-zip php7.2-intl php7.2-xsl php7.2-sqlite3 -y
apt install php7.4 php7.4-fpm php7.4-curl php7.4-gd php7.4-mysql php7.4-mbstring php7.4-zip php7.4-intl php7.4-xsl php7.4-sqlite3 -y
php -v
php7.2 -v
php7.4 -v
service php7.2-fpm status
service php7.4-fpm status
```

### Установка PHP-FPM
Открываем конфиг и настраиваем путь для статуса
```
nano /etc/php/7.x/fpm/pool.d/www.conf
```
> Содержимое для файла:
> ```
> pm.status_path = /status7x
> ```
Также находим в конфиге путь до php-fpm сокета, ключ `listen` и записываем себе в блокнотик этот путь, он пригодится при настройке в nginx
Перечитываем настройки
```
service php7.x-fpm reload
```

### Default сайт
В качестве него может выступить системный домен, выданный хостингом
```
nano /etc/nginx/sites-available/default
```
> Содержимое для файла:
> ```
> server {
>     listen 80;
>     listen [::]:80;
> 
>     server_name domain.hosting.ru;
> 
>     root /var/www/domains/domain.hosting.ru;
>     index index.php index.html
> 
>     error_page 405 =200 $request_uri;
> 
>     # Disallow all dot files
>     location ~ /\. {
>         deny all;
>         access_log off;
>         log_not_found off;
>     }
> 
>     # Location for php-fpm status
>     location /status7x {
>         auth_basic "Restricted";
>         auth_basic_user_file /var/www/domains/domain.hosting.ru/.htpasswd;
> 
>         include fastcgi_params;
>         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
>         fastcgi_pass unix:/run/php/php7.x-fpm.sock;
>     }
>     
>     # Root location
>     location / {
>         try_files $uri $uri/ /index.php$is_args$args;
> 
>         # Location for php
>         location ~ \.php$ {
>             try_files $uri =404;
> 
>             include fastcgi.conf;
>             fastcgi_pass unix:/run/php/php7.x-fpm.sock;
>             fastcgi_index index.php;
>         }        
>     }
>     
>     # Root location alternatively
>     #location / {
>     #    try_files $uri $uri/ /index.html$is_args$args;
>     #
>     #    # Location for html, css and js
>     #    location ~* ^.+\.(html|css|js)$ {
>     #        expires 60s;
>     #    }
>     #
>     #    # Location for images
>     #    location ~* ^.+\.(jpg|gif|png)$ {
>     #        expires 3d;
>     #    }
>     #}
> }
> ```
Проверяем и перечитываем конфиг:
```
nginx -t
service nginx reload
```
