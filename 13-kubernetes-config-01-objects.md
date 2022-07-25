# Домашнее задание к занятию "13.1 контейнеры, поды, deployment, statefulset, services, endpoints"

## Задание 1: подготовить тестовый конфиг для запуска приложения
![2022-07-22_19-03-33](https://user-images.githubusercontent.com/88886716/180815905-f3af7057-44b5-4797-9906-56cbb2349e87.png)

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: frontandback
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.20
        imagePullPolicy: IfNotPresent
        name: front
      - image: praqma/network-multitool:alpine-extra
        imagePullPolicy: IfNotPresent
        name: back
        env:
          - name: HTTP_PORT
            value: "8080"
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv
spec:
  storageClassName: storage
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /mr/zk
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc
spec:
  storageClassName: storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
          name: db
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
      volumes:
      - name: www
        persistentVolumeClaim:
          claimName: pvc
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
```
## Задание 2: подготовить конфиг для production окружения
![2022-07-25_18-18-13](https://user-images.githubusercontent.com/88886716/180815746-bda43f79-37b6-4659-a3c2-4c9bb1ad7bf0.png)

```
apiVersion: v1
kind: Service
metadata:
  name: front
  labels:
    app: front
spec:
  ports:
  - port: 80
    name: front
  clusterIP: None
  selector:
    app: front
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: front
  name: front
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: front
  template:
    metadata:
      labels:
        app: front
    spec:
      containers:
      - image: nginx:1.20
        imagePullPolicy: IfNotPresent
        name: front
		
apiVersion: v1
kind: Service
metadata:
  name: back
  labels:
    app: back
spec:
  ports:
  - port: 80
    name: back
  clusterIP: None
  selector:
    app: back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: back
  name: back
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: back
  template:
    metadata:
      labels:
        app: back
    spec:
      containers:
      - image: nginx:1.20
        imagePullPolicy: IfNotPresent
        name: back
		
apiVersion: v1
kind: Service
metadata:
  name: db
  labels:
    app: db
spec:
  ports:
  - port: 80
    name: db
  clusterIP: None
  selector:
    app: db
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: db
  name: db
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - image: nginx:1.20
        imagePullPolicy: IfNotPresent
        name: db
```
---
