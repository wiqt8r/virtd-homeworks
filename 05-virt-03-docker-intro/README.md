
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

## Задача 1

Образ в dockerhub-репозитории: https://hub.docker.com/r/wiqt8r/custom-nginx

## Задача 2
Скриншоты консоли, где видно все введенные команды и их вывод:
<img width="1937" height="817" alt="Docker-task2" src="https://github.com/user-attachments/assets/33f3f30b-6c49-40d1-836e-7c1b3e81f10d" />



## Задача 3
После подключения к контейнеру и нажатия комбинации Ctrl-C контейнер был остановлен, потому что Ctrl-C завершило работ основного процесса контейнера.

Скриншоты консоли, где видно все введенные команды и их вывод:
<img width="1933" height="653" alt="image" src="https://github.com/user-attachments/assets/151add81-475f-4e56-b08b-7a881c4619d1" />
<img width="1955" height="1294" alt="image" src="https://github.com/user-attachments/assets/1a6cedb3-fbce-4efc-9d02-990ff67ceff0" />
<img width="1936" height="709" alt="image" src="https://github.com/user-attachments/assets/957fa01c-0d67-46dc-a246-e00884b6b4eb" />

Объяснение созданной проблемы: Контейнер пробрасывает порт 8080 хоста на порт 80 контейнера.Я изменил конфигурацию Nginx на порт 81 внутри контейнера, но хост всё ещё пытается обращаться к порту 80 контейнера через 8080. Порт 80 не поднят, запросы не доходят, curl возвращает ошибку. Причина в несовпадении порта контейнера и порта проброса.


## Задача 4
Скриншот консоли, где видно все введенные команды и их вывод:
<img width="922" height="712" alt="image" src="https://github.com/user-attachments/assets/3657bb81-28ac-4c08-a015-a9600aa480f1" />



## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


