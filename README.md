# LEMP
Памятка php-разработчикам для самостоятельного администрирования сервера, написана исключительно для собственных нужд, поэтому вопросы о выборе того или иного инструмента не рассматриваются, принимайте как есть.

Всё, что описано ниже, полностью опробовано на моём VPS и работает в настоящий момент.

## Актуальный набор инструментов
- ОС: `Debian 10+`
- Вебсервер: `Nginx 1.18+`
- БД: `MySQL 8.0+`
- ЯП: `PHP 7.4+`
- SSL: `Let's Encrypt Wildcard`
  - DNS: `Timeweb.ru`. Для автоматического продления wildcard-сертификата необходимо динамическое управления DNS-записями посредством API. Была попытка сделать на Яндекс.Коннект, но у них медленно происходит обновление информации из-за этого скрипт продления сертификата не успевает увидеть необходимую запись в DNS.

## Содержание
- [Debian](https://github.com/DimNS/LEMP/blob/master/debian.md)
  - [logrotate](https://github.com/DimNS/LEMP/blob/master/logrotate.md)
  - [Nginx Amplify](https://github.com/DimNS/LEMP/blob/master/nginx_amplify.md)
- [MySQL](https://github.com/DimNS/LEMP/blob/master/mysql.md)
- [Nginx](https://github.com/DimNS/LEMP/blob/master/nginx.md)
  - [.htpasswd](https://github.com/DimNS/LEMP/blob/master/htpasswd.md)
- [PHP](https://github.com/DimNS/LEMP/blob/master/php.md)
  - [XHProf (tideways_xhprof)](https://github.com/DimNS/LEMP/blob/master/tideways_xhprof.md)
- [SSL](https://github.com/DimNS/LEMP/blob/master/ssl.md)
- [Подключение домена к вебсерверу](https://github.com/DimNS/LEMP/blob/master/domain.md)
- [Повседневные команды](https://github.com/DimNS/LEMP/blob/master/commands.md)
