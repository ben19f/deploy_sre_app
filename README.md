# Deploy SRE App

## Требования

- Ansible
- Debian 12


для запуска нужно внести свои сервера в inventory

## Полное развертывание

ansible-playbook -i inventory.ini playbook_full.yml

## Обновление приложения

ansible-playbook -i inventory.ini playbook_deploy.yml

## Что разворачивается

- усстановка пакетов и докера на сервер
- загрузка приложения
- PostgreSQL
- Nginx
- PHP приложение в докере



_______


🐘 PostgreSQL tuning

PostgreSQL настроен через шаблон конфигурации (postgresql.conf.j2) с динамическими параметрами:

shared_buffers → ~25% RAM
effective_cache_size → ~75% RAM
work_mem → фиксированное значение (16MB)
maintenance_work_mem → фиксированное значение (64MB)
max_connections → ограничено до 50 для предотвращения перегрузки памяти

📌 Пример (для сервера 2GB RAM):

shared_buffers: 512MB
effective_cache_size: 1536MB
⚙️ Nginx tuning

Для повышения производительности Nginx настроен:

worker_processes = auto (соответствует числу CPU cores)
оптимизирована обработка подключений под серверные ресурсы


в php-fpm возможно обратил бы внимание на лмиты (порезал бы минимум пополам)
memory_limit = 
opcache.memory_consumption = 

opcache.max_accelerated_files = 

