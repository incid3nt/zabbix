# Домашнее задание к занятию Система мониторинга Zabbix - Еноктаев Олег



---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

Требования к результатам

1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

1.2 Установка postgresql:

обновим кэш
```
apt update
```
установим postgresql
```
sudo apt install postgresql
```
проверим установился ли :
```
oleg@lvm:~$ systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; prese>
     Active: active (exited) since Sun 2025-01-05 11:41:51 UTC; 1min 17s ago
   Main PID: 13403 (code=exited, status=0/SUCCESS)
        CPU: 3ms
```
Установим репозиторий Zabbix:
```
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
dpkg -i zabbix-release_latest_7.0+debian12_all.deb
apt update
```
Установим Zabbix сервер, веб-интерфейс и агент
```
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```