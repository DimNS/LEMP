# Повседневные команды

## Ubuntu

### Быстрый запуск ранее выполняемых команд
- Нажимаем CTRL+R и жмем ENTER
- В новой строке пишем что ищем, до тех пор пока не увидим необходимую команду, и жмем ENTER, найденная команда выполняется

### Основные команды apt
- Обновить списки доступных пакетов - это самая главная команда которую необходимо выполнять практически перед любой другой командой apt, чтобы быть уверенными что работаем с актуальным списком пакетов
  ```
  sudo apt update
  ```
- Поиск пакетов по имени
  ```
  sudo apt search ИМЯ_ПАКЕТА
  ```
- Показать подробную информацию о пакете
  ```
  sudo apt show ИМЯ_ПАКЕТА
  ```
- Установить пакет
  ```
  sudo apt install ИМЯ_ПАКЕТА
  ```
- Удалить пакет
  ```
  sudo apt remove ИМЯ_ПАКЕТА
  ```
- Список пакетов для которых есть обновления
  ```
  sudo apt list --upgradable
  ```
- Обновить все пакеты для которых есть обновления
  ```
  sudo apt upgrade
  ```
- Обновление одного (нескольких) пакета (пакетов)
  ```
  sudo apt update
  sudo apt list --upgradable
  sudo apt install --only-upgrade <package>
  ```
- Полное обновление системы
  ```
  sudo apt full-upgrade
  ```
- Редактировать файл источников программного обеспечения
  ```
  sudo apt edit-sources
  ```

### Основные команды Ubuntu
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
