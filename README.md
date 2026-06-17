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