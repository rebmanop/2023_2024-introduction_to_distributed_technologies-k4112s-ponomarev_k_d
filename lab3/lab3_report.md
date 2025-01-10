# Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."
University: [ITMO University](https://itmo.ru/ru/)\
Faculty: [FICT](https://fict.itmo.ru)\
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)\
Year: 2024/2025\
Group: K4112s\
Author: Ponomarev Kirill Dmitrievich\
Lab: Lab3\
Date of create: 10.01.2025\
Date of finished: -

### 1 Пишем манифест
![image](https://github.com/user-attachments/assets/75fd8c80-498e-4f84-a75d-df3e9ca19471)


### 2 Включаем ingress аддон
```bash
minikube addons enable ingress
```

### 3 Генерируем самоподписной сертификат

Сделаем это средствами openssl с помощью команды:
```bash
$ openssl req -new -newkey rsa:4096 -x509 -sha256 -days 36
5 -nodes -out selfsigned.crt -keyout selfsigned.key
```

* `-new` указывает, что нам нужен новый сертификат.
* `-newkey rsa:4096` мы указываем, что у нас будет ключ длиной 4096 бит.
* `-x509` указывает, что мы создаем сертификат по стандарту X.509
* `-sha256` сгенерирует сертификат с использованием `sha256` суммы
* `-days 365` указывает, что срок валидности этого сертификата составит 365 дней
* `-nodes` позволит не зашифровывать приватный ключ парольной фразой
* `-out file.crt` описывает, куда будет помещен полученный сертификат
* `-keyout file.key` описывает, куда будет помещен полученный ключ

![image](https://github.com/user-attachments/assets/3e34f250-7d1a-4f9a-80ca-937b6266554d)

### 4 Добавляем сертификат в minikube
```bash
$ kubectl create secret tls secret-tls --key="selfsigned.key" --cert="selfsigned.crt"
```

### 5 Применяем манифест к кластеру

![image](https://github.com/user-attachments/assets/4cbd2677-0b42-46a4-ab73-3caa47b98b3a)

### 6 Добавим маппинг доменного имени к ip в  `/hosts`:
![image](https://github.com/user-attachments/assets/b9f038aa-6c67-48be-a93b-29792f634fb9)


### 7 Запустим тунель
```bash
$ minikube tunnel
```

### 8 Проверяем
![image](https://github.com/user-attachments/assets/e384ccf7-2d43-4c9c-8f4c-605d002fff63)

![image](https://github.com/user-attachments/assets/52689c3b-a706-49f0-b74d-69c1b9dc8a93)


### 9 Схема:
![lab3 drawio](https://github.com/user-attachments/assets/24bbd158-f4c3-4f73-a41a-98b96143b89c)

