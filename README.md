#### consul-Skillbix-2.7

```
## Перед установкой нужно обновить systemd
sudo apt-get install build-essential devscripts
sudo apt install --only-upgrade systemd


## Смотреть инфу по кластеру
consul info

## Смотреть зарегистрированные сервисы
consul catalog services

## Добавить ноду в кластер
consul join IP

## Смотреть информацио по ноде 
dig @127.0.0.1 -p8600 имя_ноды.node.consul

## Смотреть информацио по сервису 
dig @127.0.0.1 -p8600 имя_сервиса.service.consul

## Смотреть агентов
curl -v http://localhost:8500/v1/agent/members | python3 -m json.tool

```

https://russianblogs.com/article/8931547619/

https://github.com/hashicorp/consul-template
