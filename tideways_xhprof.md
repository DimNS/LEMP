# XHProf (tideways_xhprof)
\* С версии 5.0.4 добавлена поддержка PHP 8.0

### Установка
Смотрим [в репозитории](https://github.com/tideways/php-xhprof-extension/releases) номер последней версии
```
apt update
apt install graphviz -y
cd /opt/
wget https://github.com/tideways/php-xhprof-extension/releases/download/v5.0.4/tideways-xhprof_5.0.4_amd64.deb
dpkg -i tideways-xhprof*
```

### Подключение
- находим необходимый so-файл в каталоге `/usr/lib/tideways_xhprof` и подключаем в `php.ini`
  > ```
  > [xhprof]
  > extension=/usr/lib/tideways_xhprof/tideways_xhprof-7.4.so
  > xhprof.output_dir="/var/www/xhprof/reports"
  > ```
- перезапускаем php-fpm
  ```
  service php7.4-fpm reload
  ```
- проверяем
  ```
  php7.4 -i | grep 'tideways_xhprof'
  ```

### Инструмент просмотра отчётов
- скачиваем инструмент
  ```
  cd /var/www
  wget https://github.com/DimNS/LEMP/raw/master/xhprof.zip
  unzip xhprof.zip
  rm xhprof.zip
  ```
- настраиваем nginx location для просмотра в браузере
  > Содержимое для файла:
  > ```
  > server {
  >     listen 80;
  > 
  >     server_name xhprof.loc;
  >     charset utf-8;
  > 
  >     root /var/www/xhprof/xhprof_html;
  >     index index.php index.html;
  > 
  >     auth_basic "Restricted";
  >     auth_basic_user_file /var/www/xhprof/.htpasswd;
  > 
  >     # Disallow all dot files
  >     location ~ /\. {
  >         deny all;
  >         access_log off;
  >         log_not_found off;
  >     }
  > 
  >     location / {
  >         try_files $uri $uri/ /index.php?$query_string;
  >     }
  > 
  >     location ~ \.php$ {
  >         include fastcgi.conf;
  > 
  >         fastcgi_pass unix:/run/php/php7.4-fpm.sock;
  >         fastcgi_index index.php;
  > 
  >         try_files $uri =404;
  >     }
  > }
  > ```
- проверяем и перечитываем конфиг
  ```
  nginx -t
  nginx -s reload
  ```

### Использование
```
<?php
tideways_xhprof_enable(TIDEWAYS_XHPROF_FLAGS_MEMORY | TIDEWAYS_XHPROF_FLAGS_CPU);

function myApp()
{
    return 1;
}

for ($i = 1; $i < 10; $i++) {
    myApp();

    sleep(5);
}

echo 'Done!';

file_put_contents(
    '/var/www/xhprof/reports/' . uniqid() . '.myapplication.xhprof',
    serialize(tideways_xhprof_disable())
);
```
