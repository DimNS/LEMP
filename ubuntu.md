# Ubuntu

### Первая настройка
```
sudo apt update
sudo apt upgrade -y
sudo apt autoremove

sudo apt update
sudo apt install nmap htop mc composer iptables-persistent fail2ban software-properties-common net-tools zip unzip -y
```

### Очистка временных каталогов от содержимого которое не использовалось больше 30 дней
```
sudo touch /etc/tmpfiles.d/clear.conf
sudo nano /etc/tmpfiles.d/clear.conf
```
> Содержимое для файла:
> ```
> D /tmp 1777 root root 30d
> D /var/tmp 1777 root root 30d
> ```

### Меняем порт SSH
```
sudo nano /etc/ssh/sshd_config
```
> Содержимое для файла:
> ```
> Port 5101
> ```
Перезапускаем сервис `ПОСЛЕ ЭТОЙ КОМАНДЫ ВАС ВЫКИНЕТ ИЗ КОНСОЛИ И НАДО БУДЕТ СНОВА ВОЙТИ`:
```
sudo service ssh restart
```

### Настраиваем fail2ban
```
sudo nano /etc/fail2ban/jail.local
```
> Содержимое для файла:
> ```
> [ssh]
> enabled  = true
> port     = 5101
> filter   = sshd
> ## если в течении 1 часа
> findtime = 3600
> ## произведено N неудачных попыток логина
> maxretry = 5
> ## то банить IP на 24 часа
> bantime  = 86400
> ```
Активируем и перезапускаем сервис:
```
sudo systemctl enable fail2ban
sudo service fail2ban restart
```

### Настраиваем iptables
```
# Для анализа портов, доступных извне
nmap localhost -sU -sT

# Смотрим правила
iptables -L -v --line-numbers

# Разрешить уже установленные входящие соединения
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Разрешение трафика на localhost
iptables -A INPUT -i lo -j ACCEPT

# Разрешение подключения к SSH, HTTP, и SSL портам
iptables -A INPUT -p tcp --dport 5101 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Запрещаем входящие соединения
iptables --policy INPUT DROP

# Смотрим правила
iptables -L -v --line-numbers

# Для удаления правила запоминаем номер (начинаются с 1 в каждом блоке)
iptables -t filter -D INPUT 6

# Сохраняем правила и перезапускаем
netfilter-persistent save
netfilter-persistent reload

# Перезагружаем ОС и проверяем
iptables -L -v --line-numbers
```
