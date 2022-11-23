# PHP
> Обратите внимание на использование буквы `x` в нескольких местах: `x.x`, `statusxx`
> Их необходимо заменить на номер необходимой вам версии PHP

### Установка нескольких версий PHP
```
curl -sSL https://packages.sury.org/php/README.txt | bash -x
apt show php -a
apt install php7.4 php7.4-fpm php7.4-curl php7.4-gd php7.4-mysql php7.4-mbstring php7.4-zip php7.4-intl php7.4-xsl php7.4-sqlite3 -y
apt install php8.1 php8.1-fpm php8.1-curl php8.1-gd php8.1-mysql php8.1-mbstring php8.1-zip php8.1-intl php8.1-xsl php8.1-sqlite3 -y
php -v
php7.4 -v
php8.1 -v
service php7.4-fpm status
service php8.1-fpm status
```

### Настройка PHP
Для FPM и CLI
```
nano /etc/php/x.x/fpm/php.ini
nano /etc/php/x.x/cli/php.ini
```
> Содержимое для файла:
> ```
> error_reporting = E_ALL
> display_errors = Off
> log_errors = On
> error_log = /var/www/phpxx_errors.log
> ```

### Настройка PHP-FPM
Открываем конфиг и настраиваем путь для статуса
```
nano /etc/php/x.x/fpm/pool.d/www.conf
```
> Содержимое для файла:
> ```
> pm.status_path = /statusxx
> ```
Также находим в конфиге путь до php-fpm сокета, ключ `listen` и записываем себе в блокнотик этот путь, он пригодится при настройке в nginx
Перечитываем настройки
```
service phpx.x-fpm reload
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
>     location /statusxx {
>         auth_basic "Restricted";
>         auth_basic_user_file /var/www/domains/domain.hosting.ru/.htpasswd;
>
>         include fastcgi_params;
>         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
>         fastcgi_pass unix:/run/php/phpx.x-fpm.sock;
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
>             fastcgi_pass unix:/run/php/phpx.x-fpm.sock;
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
