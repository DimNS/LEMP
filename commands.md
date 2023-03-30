# Повседневные команды

- [Debian](https://github.com/DimNS/LEMP/blob/master/commands.md#debian)
  - [Быстрый запуск ранее выполняемых команд](https://github.com/DimNS/LEMP/blob/master/commands.md#быстрый-запуск-ранее-выполняемых-команд)
  - [Основные команды apt](https://github.com/DimNS/LEMP/blob/master/commands.md#основные-команды-apt)
  - [Основные команды Debian](https://github.com/DimNS/LEMP/blob/master/commands.md#основные-команды-debian)
  - [Команды для работы с сервисами (systemd)](https://github.com/DimNS/LEMP/blob/master/commands.md#команды-для-работы-с-сервисами-systemd)
  - [Другие полезные команды](https://github.com/DimNS/LEMP/blob/master/commands.md#другие-полезные-команды)
- [MySQL](https://github.com/DimNS/LEMP/blob/master/commands.md#mysql)
  - [Система](https://github.com/DimNS/LEMP/blob/master/commands.md#система)
  - [Таблицы](https://github.com/DimNS/LEMP/blob/master/commands.md#таблицы)
  - [Пользователи](https://github.com/DimNS/LEMP/blob/master/commands.md#пользователи)

## Debian

### Быстрый запуск ранее выполняемых команд
- Нажимаем CTRL+R и жмем ENTER
- В новой строке пишем что ищем, до тех пор пока не увидим необходимую команду, и жмем ENTER, найденная команда выполняется

### Основные команды apt
- Обновить списки доступных пакетов - это самая главная команда которую необходимо выполнять практически перед любой другой командой apt, чтобы быть уверенными что работаем с актуальным списком пакетов
  ```
  apt update
  ```
- Поиск пакетов по имени
  ```
  apt search ИМЯ_ПАКЕТА
  ```
- Показать подробную информацию о пакете
  ```
  apt show ИМЯ_ПАКЕТА
  ```
- Установить пакет
  ```
  apt install ИМЯ_ПАКЕТА
  ```
- Удалить пакет
  ```
  apt remove ИМЯ_ПАКЕТА
  ```
- Показать список установленных пакетов по маске
  ```
  apt list --installed | grep ИМЯ_ПАКЕТА_ИЛИ_ЧАСТИ_ПАКЕТА
  ```
- Список пакетов для которых есть обновления
  ```
  apt list --upgradable
  ```
- Обновить все пакеты для которых есть обновления
  ```
  apt upgrade
  ```
- Обновление одного (нескольких) пакета (пакетов)
  ```
  apt update
  apt list --upgradable
  apt install --only-upgrade <package>
  ```
- Полное обновление системы
  ```
  apt full-upgrade
  ```
- Редактировать файл источников программного обеспечения
  ```
  apt edit-sources
  ```

### Основные команды Debian
- Название и номер версии ОС
  ```
  lsb_release -a
  ```
- Размер содержимого каталога
  ```
  du -sh * | sort -h -r
  ```
- Быстрая запаковка каталога в архив
  ```
  tar -cf www.tar /var/www
  ```

### Команды для работы с сервисами (systemd)
- Перечитать systemd при изменении файла сервиса, добавлении нового
  ```
  systemctl daemon-reload
  ```
- Активировать новый сервис
  ```
  systemctl enable /lib/systemd/user/name.service
  ```
- Деактивировать сервис
  ```
  systemctl disable nameservice
  ```
- Посмотреть статус\запустить\перезапустить\остановить
  ```
  service nameservice status
  service nameservice start
  service nameservice restart
  service nameservice stop
  ```
- Просмотр журнала конкретного сервиса
  ```
  journalctl -u nameservice -e
  ```

### Другие полезные команды
- Посмотреть и поменять временную зону сервера
  ```
  timedatectl status
  timedatectl set-timezone UTC
  timedatectl status
  ```
- Посмотреть прослушиваемые TCP, UDP и UNIX Socket порты
  ```
  netstat -ltuxnp
  ```
  > - `l` - все открытые порты (LISTEN)
  > - `t` - по протоколу TCP
  > - `u` - по протоколу UDP
  > - `x` - по протоколу UNIX Socket
  > - `n` - без резолва IP/имён
  > - `p` - но с названиями процессов и PID-ами

## MySQL

### Система
- Отображение ошибок после запроса
  ```
  SHOW WARNINGS;
  ```
- Значения системных переменных
  ```
  SHOW VARIABLES;
  ```
- Статистика по mysqld процессам
  ```
  SHOW [FULL] PROCESSLIST;
  ```
- Общая статистика
  ```
  SHOW STATUS;
  ```
- Статистика по всем таблицам в базе
  ```
  SHOW TABLE STATUS [FROM db_name];
  ```

### Таблицы
- Список баз данных
  ```
  SHOW DATABASES;
  ```
- Список таблиц в базе
  ```
  SHOW TABLES [FROM db_name];
  ```
- Список столбцов в таблице
  ```
  SHOW COLUMNS FROM таблица [FROM db_name];
  ```
- Показать структуру таблицы в формате "CREATE TABLE"
  ```
  SHOW CREATE TABLE table_name;
  ```
- Список индексов
  ```
  SHOW INDEX FROM tbl_name;
  ```

### Пользователи
- Посмотрим на политику паролей
  ```
  SHOW GLOBAL VARIABLES LIKE 'validate_password%';
  ```
- Создание нового пользователя
  ```
  CREATE USER 'имя_пользователя'@'127.0.0.1' IDENTIFIED BY 'пароль';
  ```
  > - Выдадим права на доступ ко ВСЕМ базам
  > ```
  > GRANT ALL PRIVILEGES ON *.* TO 'имя_пользователя'@'127.0.0.1' WITH GRANT OPTION;
  > ```
  > - Выдадим права на доступ к УКАЗАННОЙ базе
  > ```
  > GRANT ALL PRIVILEGES ON имя_базы.* TO 'имя_пользователя'@'127.0.0.1';
  > ```
  > - Очищаем кеш прав
  > ```
  > FLUSH PRIVILEGES;
  > ```
- Привилегии для пользователя
  ```
  SHOW GRANTS FOR имя_пользователя [FROM db_name];
  ```
- Смотрим таблицу пользователей
  ```
  SELECT User, Host, Grant_priv FROM mysql.user;
  ```
- Удаление пользователя
  ```
  DROP USER IF EXISTS имя_пользователя;
  ```
