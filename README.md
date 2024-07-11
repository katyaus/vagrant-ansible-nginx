# vagrant-ansible-nginx

Репозиторий содержит Ansible роль Nginx и Vagrant файлы, с помощью которых разворачиваются виртуальные машины для тестирования роли.
Описание действия каждого Vagrant файла представлено ниже.

| Vagrantfile | Описание |
| ------ | ------ |
| [Ansible][VfAn] | Создание ВМ на основе образа Ubuntu для работы с Ansible. Предварительно устанавливается Python, PIP, Ansible (через PIP) и прокидывается ansible.cfg |
| [Ubuntu][VfUb] | Создание ВМ на основе образа Ubuntu для тестирования роли Nginx|

В директории Ansible/ansible находится [инвентарь][In], [плейбук][Pl] и [конфигурационный файл][Cfg] Ansible.

## Описание роли Nginx
Роль выполняет следующие действия:

| Задача роли | Описание |
| ------ | ------ |
| [main.yml][TskM] | Аккумулирование всех задач и их запуск в определенной последовательности  |
| [install.yml][TskIn] | Установка Nginx из официального репозитория |
| [start.yml][TskS] | Запуск Nginx и добавление его в автозагрузку |
| [timeout.yml][TskT] | Таймаут на соединение с 80 портом |
| [config.yml][TskC] | Конфигурация Nginx с помощью [template][TmC] и выполнение перезапуска при помощи [handler][Hd] |
| [copy_index.yml][TskCIn] | 1. Создание отдельной директории на удаленной машине с веб-сервером и копирование в нее index.html файла<br>2. Копирование index.html файла с удаленной машины на Ansible сервер в предварительно созданную директорию<br>3. Назначение на директорию и файл нужного пользователя/группы и прав доступа - 640 |
| [download_index.yml][TskDIn] | Скачивание index.html файла на Ansible сервер по URL |
| [proxy.yml][TskP] | Конфигурация Nginx как обратного прокси с помощью [template][TmP] и выполнение перезапуска при помощи [handler][Hd] |

## Описание переменных роли Nginx
Переменные [**defaults**][Df]:

| Переменная | Тип данных | Описание |
| ------ | ------ | ------ |
| nginx_dependencies |Список (List) | Пакеты, необходимые для установки Nginx из официального репозитория |
| nginx_signing_key | Строка (String) | URL-адрес, по которому находится ключ подписи для Nginx |
| nginx_port | Число (Integer) | Порт, на котором работает Nginx |
| nginx_root_dir | Строка (String) | Директория, которая используется в качестве корневого каталога Nginx |
| nginx_index | Строка (String) | Список файлов, которые Nginx ищет и использует в качестве главной страницы сайта |

Переменные [**vars**][Vr]:

| Переменная | Тип данных | Описание |
| ------ | ------ | ------ |
| nginx_repository | Строка (String) | URL-адрес репозитория Nginx для установки веб-сервера  |
| nginx_port | Число (Integer) | Порт, на котором работает Nginx |
| nginx_custom_html_dir | Строка (String) | Директория на удаленной машине для копирования в нее index.html файла |
| ansible_dir_html | Строка (String) | Директория на Ansible сервере для копирования в нее index.html файла |
| owner_index | Строка (String) | Владелец директории и index.html файла |
| group_index | Строка (String) | Группа владельца директории и index.html файла |
| ansible_dir | Строка (String) | Директория на Ansible сервере для загрузки в нее index.html файла |
| nginx_proxy_port | Число (Integer) | Порт, с которого Nginx будет перенаправлять входящие запросы |

   [VfAn]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/Vagrantfile>
   [VfUb]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ubuntu/Vagrantfile>

   [In]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/hosts.yaml>
   [Pl]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/playbook.yml>
   [Cfg]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/ansible.cfg>

   [TskM]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/main.yml>
   [TskIn]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/install.yml>
   [TskS]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/start.yml>
   [TskT]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/timeout.yml>
   [TskC]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/config.yml>
   [TskCIn]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/copy_index.yml>
   [TskDIn]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/download_index.yml>
   [TskP]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/tasks/proxy.yml>
   
   [Df]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/defaults/main.yml>
   [Vr]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/vars/main.yml>
   
   [Hd]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/handlers/main.yml>
   [TmC]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/templates/nginx.conf.j2>
   [TmP]: <https://github.com/katyaus/vagrant-ansible-nginx/blob/main/Ansible/ansible/roles/nginx/templates/proxy.conf.j2>