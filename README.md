# Deploy SRE App

## Требования

- Ansible
- Debian 12
- тут добавлю теребования к целевой машине (ориентир не менее ~3GB)


для запуска нужно внести свои сервера в inventory


## Установка зависимостей Ansible

Перед запуском плейбука:

```bash
ansible-galaxy collection install -r requirements.yml


## Полное развертывание

ansible-playbook -i inventory.ini playbook_full.yml

## Обновление приложения

ansible-playbook -i inventory.ini playbook_deploy.yml

## Что разворачивается

- установка пакетов и докера на сервер
- загрузка приложения
- PostgreSQL
- Nginx
- PHP приложение в докере





Модель работы Docker:

- PHP-FPM работает в контейнере Docker
- PostgreSQL работает на хост-машине
- Контейнер обращается к PostgreSQL через IP-адрес хоста (ansible_default_ipv4.address)
- PostgreSQL настроен на разрешение подключений из подсети Docker (172.18.0.0/16)



_______
# Лимиты

Nginx tuning

Для повышения производительности Nginx настроен:

worker_processes = auto (соответствует числу CPU cores)
оптимизирована обработка подключений под серверные ресурсы


### RAM

при подключеении к серверу делим 
35% под базу данных
30% для контейнера с PHP-FPM


### в кнтейнере 
    200мб это резерв на случай ошибок  чтобы не сработал OOM killer
    остальное делим на воркеры
