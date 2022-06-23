# Домашнее задание к занятию "12.2 Команды для работы с Kubernetes"
## Задание 1: Запуск пода из образа в деплойменте
```
root@erohin-VirtualBox:~# kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4 --replicas=2
deployment.apps/hello-node created
root@erohin-VirtualBox:~# kubectl get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   2/2     2            2           10s
root@erohin-VirtualBox:~# kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-6b89d599b9-tv5rj   1/1     Running   0          28s
hello-node-6b89d599b9-vwgnv   1/1     Running   0          28s
```

## Задание 2: Просмотр логов для разработки
```
root@erohin-VirtualBox:/cert# kubectl config view 
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Fri, 17 Jun 2022 22:56:36 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: cluster_info
    server: https://10.0.2.15:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Fri, 17 Jun 2022 22:56:36 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: context_info
    namespace: default
    user: minikube
  name: minikube
- context:
    cluster: minikube
    namespace: app-namespace
    user: user1
  name: user1-context
current-context: user1-context
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/profiles/minikube/client.crt
    client-key: /root/.minikube/profiles/minikube/client.key
- name: user1
  user:
    client-certificate: /cert/user1.crt
    client-key: /cert/user1.key

root@erohin-VirtualBox:/cert# kubectl config use-context user1-context 
Switched to context "user1-context".
root@erohin-VirtualBox:/cert# kubectl auth can-i list pods
no
root@erohin-VirtualBox:/cert# kubectl auth can-i get pods
no
root@erohin-VirtualBox:/cert# kubectl auth can-i describe pods
Warning: verb 'describe' is not a known verb
yes
root@erohin-VirtualBox:/cert# kubectl auth can-i logs pods
Warning: verb 'logs' is not a known verb
yes
root@erohin-VirtualBox:/cert# kubectl auth can-i watch pods
no

kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: app-namespace
  name: pod-reader
rules:
- apiGroups: [""]
  resources: [ pods, pods/log ]
  verbs: [ logs, describe ]

kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: app-namespace
subjects:
- kind: User
  name: user1
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

## Задание 3: Изменение количества реплик 
```
root@erohin-VirtualBox:~# kubectl scale deployment hello-node --replicas=5
deployment.apps/hello-node scaled
root@erohin-VirtualBox:~# kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-6b89d599b9-h4774   1/1     Running   0          4s
hello-node-6b89d599b9-pm79t   1/1     Running   0          4s
hello-node-6b89d599b9-qjqwx   1/1     Running   0          4s
hello-node-6b89d599b9-tv5rj   1/1     Running   0          3m43s
hello-node-6b89d599b9-vwgnv   1/1     Running   0          3m43s
```
