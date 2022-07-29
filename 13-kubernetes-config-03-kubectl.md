# Домашнее задание к занятию "13.3 работа с kubectl"
## Задание 1: проверить работоспособность каждого компонента

![2022-07-29_16-30-13](https://user-images.githubusercontent.com/88886716/181775314-4c9542b7-d69a-4855-9973-b068bb7b3c0a.png)

![2022-07-29_16-32-01](https://user-images.githubusercontent.com/88886716/181775361-22b9983e-d34b-4665-98f8-bb19ca817345.png)

![2022-07-29_16-32-51](https://user-images.githubusercontent.com/88886716/181775401-d049b08c-ccd3-4018-926c-f24367c6fc21.png)

![2022-07-29_16-41-18](https://user-images.githubusercontent.com/88886716/181775480-ce490a59-4575-4d33-a7b7-640948513b7c.png)

![2022-07-29_16-42-12](https://user-images.githubusercontent.com/88886716/181775520-8db7cfe6-cafc-4219-af79-79cf833e97df.png)

![2022-07-29_16-43-26](https://user-images.githubusercontent.com/88886716/181775561-1ec9e1c5-7f7a-4c35-b8ec-c71e5c532eeb.png)

## Задание 2: ручное масштабирование

![2022-07-29_16-46-32](https://user-images.githubusercontent.com/88886716/181775693-9cee9837-11d2-4d73-96a6-106191c6ae6b.png)

```
erohin@erohin-VirtualBox:~/kubespray$ kubectl get pods -o wide
NAME                     READY   STATUS    RESTARTS   AGE     IP               NODE    NOMINATED NODE   READINESS GATES
back-6ccd55447-mtvhh     1/1     Running   0          6m15s   10.233.102.139   node1   <none>           <none>
back-6ccd55447-ngltr     1/1     Running   0          3d22h   10.233.75.6      node2   <none>           <none>
back-6ccd55447-vcnhj     1/1     Running   0          6m15s   10.233.75.8      node2   <none>           <none>
db-646cb69747-5rwfz      1/1     Running   0          3d22h   10.233.102.137   node1   <none>           <none>
front-5d555bbb57-srxf6   1/1     Running   0          6m26s   10.233.102.138   node1   <none>           <none>
front-5d555bbb57-tdncp   1/1     Running   0          3d22h   10.233.102.136   node1   <none>           <none>
front-5d555bbb57-zwsg7   1/1     Running   0          6m26s   10.233.75.7      node2   <none>           <none>


erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod back-6ccd55447-mtvhh 
Name:         back-6ccd55447-mtvhh
Namespace:    default
Priority:     0
Node:         node1/10.128.0.9

erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod back-6ccd55447-ngltr 
Name:         back-6ccd55447-ngltr
Namespace:    default
Priority:     0
Node:         node2/10.128.0.4

erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod back-6ccd55447-vcnhj 
Name:         back-6ccd55447-vcnhj
Namespace:    default
Priority:     0
Node:         node2/10.128.0.4


erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod front-5d555bbb57-srxf6
Name:         front-5d555bbb57-srxf6
Namespace:    default
Priority:     0
Node:         node1/10.128.0.9

erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod front-5d555bbb57-tdncp 
Name:         front-5d555bbb57-tdncp
Namespace:    default
Priority:     0
Node:         node1/10.128.0.9

erohin@erohin-VirtualBox:~/kubespray$ kubectl describe pod front-5d555bbb57-zwsg7 
Name:         front-5d555bbb57-zwsg7
Namespace:    default
Priority:     0
Node:         node2/10.128.0.4
```
![2022-07-29_16-52-41](https://user-images.githubusercontent.com/88886716/181775861-3c74a8a7-a412-4479-9f4d-21af0263cb1c.png)
