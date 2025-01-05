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
Далее мы создаем пользователя в базе данных "zabbix" под пользователем postgres и создаем бд "zabbix"
```
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```
На хосте Zabbix сервера импортируйте начальную схему и данные:
```
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
Настроим базу данных для Zabbix сервера
Отредактируем файл /etc/zabbix/zabbix_server.conf внесем пароль:
```
DBPassword=password
```
Запустим процессы Zabbix сервера и агента и настроим их запуск при загрузке ОС.
```
 systemctl restart zabbix-server zabbix-agent apache2
 systemctl enable zabbix-server zabbix-agent apache2
```
Пора идти смотреть в веб интерфейс заббикс.
Похоже что все ок!
![zabbix](https://github.com/incid3nt/zabbix/blob/main/img/chrome_lIEaG6KcVT.png)
![zabbix](https://github.com/incid3nt/zabbix/blob/main/img/chrome_Qx5GQN7os2.png)
![zabbix](https://github.com/incid3nt/zabbix/blob/main/img/chrome_1aRlnI60pS.png)
![zabbix](https://github.com/incid3nt/zabbix/blob/main/img/chrome_G6jja8yrRv.png)

---

Задание 2

Установите Zabbix Agent на два хоста.

Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

- Требования к результаты
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

2.2 Для установки zabbix-agent введем команду:
```
apt install zabbix-agent
```
Вводим команду:
```
sed -i 's/Server=127.0.0.1/Server=192.168.1.27/g' /etc/zabbix/zabbix-agentd.conf
```
там где 192.168.1.27 указываем ип адрес zabbix сервера и перезапускаем агента и его так же нужно добавить в автозагрузку:
```
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
и 