University: [ITMO University](https://itmo.ru/ru/) \
Faculty: [FICT](https://fict.itmo.ru) \
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies) \
Year: 2024/2025 \
Group: K4111c \
Author: Ponomarev Kirill Dmitrievich \
Lab: Lab4 \
Date of create: 16.01.2025 \
Date of finished: -


## Лабораторная работа №4 "Сети связи в Minikube, CNI и CoreDNS"

### 1. Запускаем миникуб с плагином calico и 2мя нодами
```
minikube start --network-plugin=cni --cni=calico --nodes 2 -p multinode
```
 - --network-plugin=cni - говорим Minikube использовать плагин сети Container Network Interface
 - --cni=calico - указываем конкретный плагин Calico
 - --noodes 2 - указываем количество нод
![image](https://github.com/user-attachments/assets/086e0d87-48dd-4fab-8b90-193c3e6a1865)


### 2. Удаляем дефолтный ippool
```
kubectl delete ippools default-ipv4-ippool
```

### 3. Помечаем ноды индексами 0 и 1
```
kubectl label node multinode rack=0
kubectl label node multinode-m02 rack=1
```
### 4. Применяем написанный для пулов манифест

```
kubectl-calico apply -f ippool.yaml --allow-version-mismatch
```

### 5. Повторяем работу, проделанную во второй лабораторной работе по созданию деплоймента и сервиса.

![image](https://github.com/user-attachments/assets/c82870f7-bd41-4e9a-ab65-5524b88dca82)

### 6. Пробрасываем порты
```
kubectl port-forward service/frontend-service 8080:8080
```

### 7. Полученый результат в браузере:

![image](https://github.com/user-attachments/assets/5b3e7384-cb45-4d87-942f-285807730b46)


### 8. Пингуем первый под, получаем его адрес
```
kubectl exec frontend-deployment-67d4686d7f-6bb6w -- nslookup 192.168.0.193
```

![image](https://github.com/user-attachments/assets/e885c70f-dd39-4f68-84b1-fceb187a8706)

```
kubectl exec frontend-deployment-67d4686d7f-6bb6w -- ping 192-168-0-193.frontend-service.default.svc.cluster.local
```

### 9. Пингуем второй под:
```
kubectl exec frontend-deployment-67d4686d7f-xrwd5 -- ping 192.168.1.128
```
![image](https://github.com/user-attachments/assets/0255e450-6cc7-467d-ab51-0bbbc659724b)

### 10. Схема

![image](https://github.com/user-attachments/assets/b9d3e97b-202f-4c7c-ae2c-fffa35a051e1)

