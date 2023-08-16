# Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки»

### Задание 1
- Запустите два simple python сервера на своей виртуальной машине на разных портах
- Установите и настройте HAProxy, воспользуйтесь материалами к лекции по [ссылке](2/)
- Настройте балансировку Round-robin на 4 уровне.
- На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

#### Ответ:

```
listen web_tcp

        bind :1325
        balance     roundrobin
        server s1 127.0.0.1:8888 check inter 3s
        server s2 127.0.0.1:9999 check inter 3s
```
![](https://github.com/qqb8/hw-netology/blob/main/A.2.%20screen1.png)

---

### Задание 2
- Запустите три simple python сервера на своей виртуальной машине на разных портах
- Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
- HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
- На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

#### Ответ:

```
frontend main
    mode http
    bind :8088
    acl acl_example.com hdr(host) -i example.local
    use_backend app if acl_example.com

backend app
    balance     roundrobin
    server  s1 127.0.0.1:8888 check weight 2
    server  s2 127.0.0.1:9999 check weight 3
    server  s3 127.0.0.1:7777 check weight 4
```
![](https://github.com/qqb8/hw-netology/blob/main/A.2.%20screen2.png)