# Ansible-sch1799

Коллекция ролей и плейбуков [Ansible](https://www.ansible.com/),
управляющая конфигурацией серверов и приложений Школы № 1799.

## Серверы

* VPS в интернете
* Proxmox в зданиях школ
* Виртуальные машины на Proxmox
* Виртуальные машины МЭШ

## Приложения

* WireGuard VPN
* Nginx Proxy Manager
* GLPI
* NMS (LibreNMS, SmokePing)
* Microsoft Deployment Toolkit

## Зачем это нужно

Результатом запуска основного плейбука `main.yml` является приведение конфигурации серверов, указанных в Inventory-файле `inventories/hosts`, в соответствие с определенными для них ролями. Повторные запуски проверяют и применяют конфигурацию в случае обнаружения изменений, а также устанавливают обновления пакетов ОС и образов Docker при наличии новых версий в соответствующих источниках.

Это означает, что ручное изменение в консоли сервера описанных здесь параметров будет отменено при повторном запуске плейбука. Другими словами, нельзя, например, зайти на сервер и добавить что-нибудь в файл docker-compose.yml, т.к. файл не будет соответствовать таковому в Ansible, и будет перезаписан. По-тихому запустить контейнер командой docker run или установить пакет командой apt install можно (но не нужно!), т.к.

> конфигурация сервера как единого целого (с установленными пакетами, их версиями, запущенными процессами и т.д.) не "заморожена", не отслеживается и нигде не хранится. Предполагается, что всё, что явно не настроено с помощью Ansible, имеет настройки по-умолчанию.

Любые изменения, которые необходимо применить к группе серверов или отдельному серверу, нужно вносить в конфигурацию Ansible, дополняя её новыми ролями и плейбуками или изменяя существующие, с последующим тестированием.

## Структура

1. Файл `main.yml` предназначен для запуска плейбуков Ansible:
    * Файл `vps.yml` - конфигурация VPS.
    * Файл `pve.yml` - конфигурация Proxmox.
    * Файл `vms.yml` - конфигурация виртуальных машин.
    * Файл `services.yml` - установка и настройка приложений.
1. Папка `inventories` содержит Inventory-файлы Ansible, разделенные по окружению (тестовое, рабочее).
1. Папка `roles` содержит роли Ansible, каждая из которых отвечает за отдельную часть конфигурации, обозначенную в названии папки и в файле `README.md` внутри.

## Как получить стандартно настроенный сервер без приложений

1. Клонировать репозиторий проекта или скачать и распаковать архив на любую машину с Linux или WSL
1. Установить [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) по инструкции для соответствующего дистрибутива
1. Установить `sshpass`
1. Установить с нуля из ISO или клонировать из шаблона виртуальную машину с чистой Debian 10/11 и настроенным доступом через **SSH** для пользователя **root** с паролем или по ключу
1. Перейти в корневую папку проекта
1. Внести DNS-имя или IP-адрес виртуальной машины в файл `inventories/hosts`
1. Установить внешние зависимости командой `ansible-galaxy install -r requirements.yml`
1. Запустить плейбук командой `ansible-playbook vms.yml -u root -k -v`
    * Параметр `-k` отвечает за запрос на ввод пароля пользователя, указанного после `-u`
    * Параметр `-v` отвечает за вывод подробных сообщений в консоль
