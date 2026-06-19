# Deploy SRE App

## Требования

- Ansible
- Debian 12
- тут добавлю теребования к целевой машине (ориентир не менее ~3GB)


Скачайте этот репозиторий    
git clone https://github.com/ben19f/deploy_sre_app.git

зайдите в него 
cd ~/deploy_sre_app


## Перед запуском плейбука:

### Установка зависимостей Ansible

ansible-galaxy collection install -r requirements.yml

### Установка и шифрование секретов

запишите пароль БД 
nano group_vars/vault.yml

(вместо sre запишите свое значение)


зашифруйте пароль коммандой
ansible-vault encrypt group_vars/vault.yml
(спросит пароль шифрования - придумайте свой пароль НЕ ПАРОЛЬ БД)

### внесение своих переменных

для запуска нужно внести свои сервера в inventory
nano  inventory.ini

для запуска нужно внести свои названия БД и имя юзера БД
nano group_vars/all.yml


## Полное развертывание

ansible-playbook -i inventory.ini playbook_full.yml



## Что разворачивается

- установка пакетов и докера на сервер
- загрузка приложения
- PostgreSQL
- Nginx
- PHP приложение в докере



## Обновление приложения

ansible-playbook -i inventory.ini playbook_deploy.yml







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




## структура устанавливаемого приложения
https://github.com/muxx/sre-hello-world.git
sre-hello-world
├── docker
│   ├── hosts
│   │   ├── app.conf
│   │   └── app-defaults
│   └── images
│       ├── php
│       │   ├── Dockerfile
│       │   └── php.ini
│       └── postgres
│           └── init-extensions.sh
├── docker-compose.yml
├── LICENSE
├── README.md
└── web
    ├── app.php
    └── title.jpg
