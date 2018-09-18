# dev-docker-sample

Это типовая настройка для Docker для быстрого развертывания окружения разработки на: php, js, mysql, postgres, nginx.

1. `apt-get update`
2. `apt-get install git`
3. `git clone https://github.com/d-a-gerashenko/dev-docker-sample ./some_project_dir`
4. `cd ./some_project_dir`
5. `apt-get install docker-compose`
6. `chmod 777 -R ./commands/`
7. Редактируем **./.env**
8. `./commands/containers/start.sh`

На 80 порту работает nginx+fpm. На 8080 adminer. Проверяем, что они открываются.

MySQL:
 - host: mysqldb
 - user: root
 - password: 123

Postgres:
 - host: postgresdb
 - user: postgres
 - password: 123

Composer уже установлен. Если нужно выполить какие-либо php конманды, то их можно определить в файле **./.env**.

Есть отдельный контейнер для Node. Список команд определяется в файле **./.env**.

Настройки и файлы контейнеров лежат в **./services/**. Там же храняется файлы Dockerfile, в которых определяется образ и его версия, из которого создается контейнер.

Для баз данных директория с данными базы вынесена из контейнера, т.е. если контейнер удалить, то данные останутся. Все это можно найти в **./services/**.

Команды управления лежат в **./commands/**. Если хотим вызвать команду, то из корнейво директории (**./some_project_dir**) пишем `./c` нажимаем **Tab** 3 раза, дописываем продолжение и т.д. В **./commands/containers/** лежат общие команды управления контейнерами (reset.sh - стоит использовать, если файлы Docker-а не правились, restart.sh - перезапускает и, при необхоидмости, пересоздает контейнеры).

В **./commands/in/** лежат файлы помогающие выполнять комманды внутри контейнеров. **./commands/in/container.sh** - базовый файл для этой ветки, его используют все другие файлы из этой ветки. На вход **./commands/in/container.sh** принимает название контейнера, а далее идет команда, например:
```
./commands/in/container.sh phpfpm php -r "echo 'hi'.PHP_EOL;"
```
предыдующую команду можно упростить использую более специализированную команду из той же ветки:
```
./commands/in/phpfpm.sh php -r "echo 'hi'.PHP_EOL;"
```
и еще упростить:
```
./commands/in/phpfpm/php.sh -r "echo 'hi'.PHP_EOL;"
```
При дописывании собственных команд следует придерживаться этой структуры.

Чтобы открыть консоль контейнера php (node) используем команду: `./commands/in/phpfpm/bash.sh` (`./commands/in/node/bash.sh`).
