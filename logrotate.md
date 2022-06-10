# logrotate

### Установка
```
sudo apt update
sudo apt install logrotate -y
```

### Настройка
```
sudo touch /etc/logrotate.d/www
sudo nano /etc/logrotate.d/www
```
> Содержимое для файла:
> ```
> /var/www/nginx_access.log {
>   weekly
>   compress
>   delaycompress
>   rotate 2
>   missingok
>   nocreate
>   sharedscripts
>   postrotate
>     test ! -f /var/run/nginx.pid || kill -USR1 `cat /var/run/nginx.pid`
>   endscript
> }
>
> /var/www/nginx_error.log {
>   daily
>   compress
>   delaycompress
>   rotate 2
>   missingok
>   nocreate
>   sharedscripts
>   postrotate
>     test ! -f /var/run/nginx.pid || kill -USR1 `cat /var/run/nginx.pid`
>   endscript
> }
>
> /var/www/php72_errors.log {
>   daily
>   compress
>   delaycompress
>   rotate 2
>   missingok
>   notifempty
>   create 644 www-data www-data
>   sharedscripts
>   postrotate
>     service php7.2-fpm flush-logs > /dev/null
>   endscript
> }
>
> /var/www/php74_errors.log {
>   daily
>   compress
>   delaycompress
>   rotate 2
>   missingok
>   notifempty
>   create 644 www-data www-data
>   sharedscripts
>   postrotate
>     service php7.4-fpm flush-logs > /dev/null
>   endscript
> }
> ```

### Перезапускаем сервис
```
sudo service logrotate restart
```
