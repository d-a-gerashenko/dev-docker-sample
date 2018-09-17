# dev-docker-sample

Это типовая настройка для Docker для быстрого развертывания окружения разработки на: php, js, mysql, postgres, nginx.

1. `apt-get update`
2. `apt-get install git`
3. `git clone https://github.com/d-a-gerashenko/dev-docker-sample ./some_project_dir`
4. `cd ./some_project_dir`
5. `apt-get install docker-compose`
6. `chmod 777 -R ./commands/`
7. `./commands/containers/start.sh`

На 80 порту работает nginx+fpm. На 8080 adminer. Проверяем, что они открываются.

Путь к frontend и backend директориям задается через символические ссылки, которые лежат в директории **./links/**. По умолчанию обе ссылки ведут в директорию **./project/**.

Чтобы изменить ссылки нужно сначала остановить контейнеры `./commands/containers/stop.sh`, затем удаляем старую ссылку `rm ./links/frontend` (`rm ./links/backend`) и создаем новую командой `ln -s ./path/to/dir/ ./links/frontend` (`ln -s ./path/to/dir/ ./links/backend`). Проверить пути ссылок можно командой `ls -la ./links/`.

./path/to/dir - путь к исходным кодам фроненда или бекенда.

В комплекте идет MySQL и Postgres.

MySQL:
 - host: mysqldb
 - user: root
 - password: 123

Postgres:
 - host: postgresdb
 - user: postgres
 - password: 123

Есть отдельный контейнер для Node.

Настройки и файлы контейнеров лежат в **./services/**. Там же храняется файлы Dockerfile, в которых определяется образ и его версия, из которого создается контейнер.

Для баз данных директория с данными базы вынесена из контейнера, т.е. если контейнер удалить, то данные останутся. Все это можно найти в **./services/**.

Команды управления лежат в **./commands/**. Если хотим вызвать команду, то из корнейво директории (**./some_project_dir**) пишем `./c` нажимаем **Tab** 3 раза, дописываем продолжение и т.д. В **./commands/containers/** лежат общие команды управления контейнерами (reset.sh - стоит использовать, если файлы Docker-а не правились, restart.sh - перезапускает и, при необхоидмости, пересоздает контейнеры).

В **./commands/exec-in/** лежат файлы помогающие выполнять комманды внутри контейнеров. **./commands/exec-in/container.sh** - базовый файл для этой ветки, его используют все другие файлы из этой ветки. На вход **./commands/exec-in/container.sh** принимает название контейнера, а далее идет команда, например:
```
./commands/exec-in/container.sh phpfpm php -r "echo 'hi'.PHP_EOL;"
```
предыдующую команду можно упростить использую более специализированную команду из той же ветки:
```
./commands/exec-in/phpfpm.sh php -r "echo 'hi'.PHP_EOL;"
```
и еще упростить:
```
./commands/exec-in/phpfpm/php.sh -r "echo 'hi'.PHP_EOL;"
```
При дописывании собственных команд следует придерживаться этой структуры.
