# MySQL

### Установка
```
apt update
apt policy mysql-server
apt install mysql-server -y
service mysql status
mysql_secure_installation
```

### Глобальные настройки
```
nano /etc/mysql/my.cnf
```
> Содержимое для файла:
> ```
> [mysqld]
> # Отключение резолвинга доменных имён в ip-адреса позволит поднять производительность до 20%
> skip-name-resolve
> 
> # Если приложение и сервер базы данных находятся на одном сервере
> # используйте socket-соединение, а не TCP
> # это позволит сократить время ответа до 30%
> skip-networking
> socket=/tmp/mysql.sock
> 
> # Настраиваем таймаут, дело в том что по умолчанию стоит 28800 секунд
> wait_timeout = 30
> interactive_timeout = 30
> ```

### Перезапускаем сервис
```
service mysql restart
```

### Теперь для подключения к mysql в консоли надо писать так
```
mysql --socket=/tmp/mysql.sock
```
