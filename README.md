#### consul-Skillbix-2.7

Задача
Создавать кластеры и автоматизировать их конфигурирование.

Необходимо создать HA consul кластер из трёх серверов и consul-template для управления кластером из балансировщика нагрузки и несколькими бэкендами.
Можно использовать виртуальные серверы на локальной машине или у любого провайдера.

1. С помощью Ansible Consul установите кластер на машины, включая клиенты.
Используйте репозиторий, который рассматривали по этой теме:

- $ apt install software-properties-common
- $ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
- $ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
- $ sudo apt-get update && sudo apt-get install consul-template

2. Создайте Ansible play, который будет использовать роли и развернёт кластер Consul с тремя нодами в одном дата-центре и двумя клиентами. Посмотреть это можно на схеме.Также на клиенте установите nginx и зарегистрируйте сервисы be_version_v1 и be_version_v2. Роли для Ansible можно взять те, которые использовались использовали на занятии. 
3. С помощью Ansible установите consul-template на хост, где установлен балансировщик Nginx. Рекомендуем создать роль для Ansible и добавить в неё systemd unit, создание каталогов и другие необходимые шаги, описанные в теме по consul-template. Для установки используйте модуль apt для Ubuntu или Debian. 
4. Сконфигурируйте consul-template для рендеринга шаблона в конфиг Nginx.
5. Создайте шаблон для consul-template, который создаст upstream в зависимости от того, чему равна переменная version в Ansible. Например, нужно создать upstream be_version_v1, если version равна v1 и be_version_v2, если v2     
6. Переменную version необходимо искать в KV-хранилище. По умолчанию значение переменной должно быть v2.
Location в Nginx также использует значение ключа version из KV и перенаправляет запросы в соответствующий upstream. Например, location /v2/.

#### Советы и рекомендации
Для поиска в KV store в Ansible должен быть пакет python-consul. Установите его на машину Ansible перед запуском.
Попробуйте добавить ключ version в consul KV разные значения:
- consul kv put version v1,
- consul kv put version v2,
- consul kv put version abc.


```
## Копируем ssh-ключ на сервера
ssh-copy-id sergey@192.168.1.90
...

## Перед установкой нужно обновить systemd
sudo apt-get install build-essential devscripts
sudo apt install --only-upgrade systemd

ansible-playbook -K -i consul.inv site.yml

## Меняем версию ключа
consul kv put version v1

## После внесения изменения version в consul запускаем ...
ansible-playbook -K -i consul.inv nginx-consul-template-ansible.yaml

## Смотреть инфу по кластеру
consul info

## Смотреть зарегистрированные сервисы
consul catalog services

## Добавить ноду в кластер
consul join IP

## Смотреть что есть в consul по ключу version
consul kv get version

## Смотреть информацио по ноде 
dig @127.0.0.1 -p8600 имя_ноды.node.consul

## Смотреть информацио по сервису 
dig @127.0.0.1 -p8600 имя_сервиса.service.consul

## Смотреть агентов
curl -v http://localhost:8500/v1/agent/members | python3 -m json.tool

```

https://russianblogs.com/article/8931547619/

https://github.com/hashicorp/consul-template
