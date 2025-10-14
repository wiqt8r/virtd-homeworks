
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

## Задача 1

Образ в dockerhub-репозитории: https://hub.docker.com/r/wiqt8r/custom-nginx

## Задача 2
Скриншоты консоли, где видно все введенные команды и их вывод:

<img width="1937" height="817" alt="Docker-task2" src="https://github.com/user-attachments/assets/33f3f30b-6c49-40d1-836e-7c1b3e81f10d" />



## Задача 3
После подключения к контейнеру и нажатия комбинации `Ctrl-C` контейнер был остановлен, потому что комбинация `Ctrl-C` завершила работу основного процесса контейнера.

Скриншоты консоли, где видно все введенные команды и их вывод:

<img width="1933" height="653" alt="image" src="https://github.com/user-attachments/assets/151add81-475f-4e56-b08b-7a881c4619d1" />
<img width="1955" height="1294" alt="image" src="https://github.com/user-attachments/assets/1a6cedb3-fbce-4efc-9d02-990ff67ceff0" />
<img width="1936" height="709" alt="image" src="https://github.com/user-attachments/assets/957fa01c-0d67-46dc-a246-e00884b6b4eb" />

Объяснение созданной проблемы: Контейнер пробрасывает порт 8080 хоста на порт 80 контейнера. Я изменил конфигурацию Nginx на порт 81 внутри контейнера, но хост всё ещё пытается обращаться к порту 80 контейнера через 8080. Порт 80 не поднят, запросы не доходят, curl возвращает ошибку. Причина в несовпадении порта контейнера и порта проброса.


## Задача 4
Скриншот консоли, где видно все введенные команды и их вывод:

<img width="922" height="712" alt="image" src="https://github.com/user-attachments/assets/3657bb81-28ac-4c08-a015-a9600aa480f1" />

Запускаю контейнеры с `tail -f /dev/null`, т.к. в контейнере должен работать какой-то процесс, чтобы контейнер сразу же не завершился.


## Задача 5

При выполнении `docker compose up -d` запускается файл с каноническим именем compose.yaml, при этом docker-compose.yaml игнорируется.
Запустить оба файла можно, добавив в compose.yaml директиву `include`:
```
include:
  - docker-compose.yaml
```

После удаления манифеста compose.yaml и выполнения `docker compose up -d` возникло предупреждение `WARN[0000] Found orphan containers`. Docker compose запомнил, что раньше в этом проекте запускался контейнер portainer из файла compose.yaml. Этот файл удалён, но контейнер portainer всё ещё существует. Теперь при запуске Compose видит, что контейнер portainer сделался "orphan", т.е. он не описан ни в одном из существующих yaml-файлов. Удалить не описанные в yaml контейнеры можно при помощи опции `--remove-orphans`.
Также есть WARN с текстом `the attribute version is obsolete`, возникает из-за того, что в yaml есть строка `version: "3"`, которая не требуется для работы актуальной версии Docker Compose. Это не ошибка, а предупреждение, поэтому можно удалить эту строку из yaml-файла, а можно оставить.

Скриншоты консоли:

<img width="1955" height="1294" alt="image" src="https://github.com/user-attachments/assets/7d154a7c-8fcb-4c72-96db-60d91a8073aa" />
<img width="1955" height="1294" alt="image" src="https://github.com/user-attachments/assets/8d42894c-ef84-4413-8eec-3e0f223909ae" />
<img width="1932" height="1084" alt="image" src="https://github.com/user-attachments/assets/9ffcdef2-ec89-4c4a-bd37-9116d623cbac" />
<img width="1932" height="395" alt="image" src="https://github.com/user-attachments/assets/20d96144-f389-4a00-aec8-e937603ca2e0" />

compose.yaml

<img width="641" height="400" alt="image" src="https://github.com/user-attachments/assets/05f686d2-39ff-4b2f-900e-21601bccb402" />

Portainer:

<img width="1426" height="1366" alt="image" src="https://github.com/user-attachments/assets/061aa3dc-51bc-4ce1-82f7-dc38aa409102" />

---




