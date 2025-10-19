# Домашнее задание к занятию 5. «Практическое применение Docker»

### Инструкция к выполнению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
3. **Своё решение к задачам оформите в вашем GitHub репозитории.**
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
5. Сопроводите ответ необходимыми скриншотами.

---
## Примечание: Ознакомьтесь со схемой виртуального стенда [по ссылке](https://github.com/netology-code/shvirtd-example-python/blob/main/schema.pdf)

---

## Задача 0

<img width="969" height="133" alt="image" src="https://github.com/user-attachments/assets/69d8b1df-d93c-4a4b-9126-343b4364870d" />


---

## Задача 1

Dockerfile.python:

<img width="955" height="421" alt="image" src="https://github.com/user-attachments/assets/a4670c16-0137-44eb-841c-a4b3f7cb5ef7" />

.dockerignore:

<img width="346" height="360" alt="image" src="https://github.com/user-attachments/assets/88546bd5-226a-4d6c-a5eb-86723f203110" />

Задачи поднять MySQL на этом этапе не было, контейнер работает, из браузера достуапен:

<img width="1066" height="154" alt="image" src="https://github.com/user-attachments/assets/bfeb7bc5-8692-4ed5-91a4-4e4be664ee36" />

(*)Web-приложение без использования docker, с помощью venv. (Mysql в контейнере):

<img width="1297" height="231" alt="image" src="https://github.com/user-attachments/assets/a27a48d1-0b36-49a2-bffd-79c39e8f0525" />

---

## Задача 2 (*)
Результат сканирования в убунту:

<img width="1791" height="313" alt="image" src="https://github.com/user-attachments/assets/9d496a52-f9b4-4fd9-819e-9cf6a482c05a" />

И в интерфейсе Яндекс клауда:

<img width="1323" height="814" alt="image" src="https://github.com/user-attachments/assets/1eae0174-9ed9-4497-852d-527214020700" />



## Задача 3

Содержимое compose.yaml (пришлось добавить healthcheck в контейнере db и задержку при старте веба, иначе таблица не успевала создаваться, и веб ругался, что Table 'virtd.requests' doesn't exist):
```
include:
  - proxy.yaml

services:
  db:
    image: mysql:8
    container_name: mysql-container
    restart: always
    env_file:
      - .env
    networks:
      backend:
        ipv4_address: 172.20.0.10
    volumes:
      - mysql_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u${MYSQL_USER}", "-p${MYSQL_PASSWORD}"]
      interval: 5s
      timeout: 3s
      retries: 10
      start_period: 10s

  web:
    image: cr.yandex/crpvp46miuir2eoc750g/my-python-app:v1
    container_name: web
    restart: always
    env_file:
      - .env
    networks:
      backend:
        ipv4_address: 172.20.0.5
    depends_on:
      db:
        condition: service_healthy
    command: >
      sh -c "sleep 15 && uvicorn main:app --host 0.0.0.0 --port 5000"

volumes:
  mysql_data:

networks:
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/24
```
файл .env
```
# Для main.py
DB_HOST=db
DB_PORT=3306
DB_USER=app
DB_PASSWORD=xxxx
DB_NAME=virtd
TABLE_NAME=requests

# Для MySQL
MYSQL_ROOT_PASSWORD=xxxx
MYSQL_HOST=db
MYSQL_PORT=3306
MYSQL_USER=app
MYSQL_PASSWORD=xxxx
MYSQL_DATABASE=virtd
TABLE_NAME=requests
```

Скриншот из БД:
<img width="1006" height="1291" alt="image" src="https://github.com/user-attachments/assets/c7267fb9-e95b-4dff-abfa-860b8cacdee2" />


## Задача 4
Bash-скрипт в репозитории: https://github.com/wiqt8r/shvirtd-example-python/blob/main/netology_test.sh

**Ссылка на fork:** https://github.com/wiqt8r/shvirtd-example-python/tree/main

Проверка сервиса на check-host:

<img width="1054" height="1113" alt="image" src="https://github.com/user-attachments/assets/55181de9-10a1-4344-8e35-5aca377e2e85" />

Настроен remote ssh context к новому серверу:

<img width="1931" height="470" alt="image" src="https://github.com/user-attachments/assets/01ba8a96-646f-4e37-8e80-7847ad733f4e" />

SQL-запрос:

<img width="954" height="871" alt="image" src="https://github.com/user-attachments/assets/e13c3fa2-ae0c-4235-bc2a-ee033c1da784" />



## Задача 5 (*)
Скрипт бэкапа MySQL: https://github.com/wiqt8r/shvirtd-example-python/blob/main/db_backup.sh

Задача в кронтабе:

```
* * * * * /opt/shvirtd-example-python/db_backup.sh >> /var/log/db_backup.log 2>&1
```

Бэкапы на сервере:

<img width="1683" height="139" alt="image" src="https://github.com/user-attachments/assets/487fee34-508e-4688-9b31-8e5797c04919" />


## Задача 6
Скриншот из Dive:

<img width="1955" height="1294" alt="image" src="https://github.com/user-attachments/assets/f0769827-7d9e-415a-8302-0b0e5ac83727" />

Извлеченный terraform:

<img width="1933" height="1090" alt="image" src="https://github.com/user-attachments/assets/f0b85b11-613e-44ea-8ea8-e416957eb95e" />



## Задача 6.1
Через docker cp:

<img width="1932" height="627" alt="image" src="https://github.com/user-attachments/assets/bbb94ae6-8342-4f7b-a9f0-cad7e042c53c" />


## Задача 6.2 (**)

Докерфайл:

<img width="537" height="132" alt="image" src="https://github.com/user-attachments/assets/0120133e-8461-4047-aaca-1ff57578525e" />

Процесс извлечения

<img width="1932" height="712" alt="image" src="https://github.com/user-attachments/assets/d00bba66-ac23-4e22-82f9-b0ecd49b7211" />


## Задача 7 (***)
*Запустите ваше python-приложение с помощью runC, не используя docker или containerd.*
*Предоставьте скриншоты  действий .*
