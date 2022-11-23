# MySQL

### Установка
```
apt update
apt install gnupg
https://dev.mysql.com/downloads/repo/apt/
cd /opt
wget https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
dpkg -i mysql-apt-config*
apt update
apt policy mysql-server
apt install mysql-server -y
service mysql status
```

### Глобальные настройки
```
sudo nano /etc/mysql/my.cnf
```
> Содержимое для файла:
> ```
> [client]
> socket = /var/run/mysqld/mysqld.sock
>
> [mysqld]
> # Отключение резолвинга доменных имён в ip-адреса позволит поднять производительность до 20%
> skip-name-resolve
>
> # Если приложение и сервер базы данных находятся на одном сервере
> # используйте socket-соединение, а не TCP
> # это позволит сократить время ответа до 30%
> skip-networking
> socket = /var/run/mysqld/mysqld.sock
> port = 3306
>
> # Настраиваем таймаут, дело в том что по умолчанию стоит 28800 секунд
> wait_timeout = 30
> interactive_timeout = 30
> ```

### Перезапускаем сервис
```
sudo service mysql restart
```

### Полезные ссылки
[Как устранить ошибки сокета в MySQL](https://www.digitalocean.com/community/tutorials/how-to-troubleshoot-socket-errors-in-mysql)
