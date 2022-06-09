# Домашнее задание к занятию "12.1 Компоненты Kubernetes"

## Задача 1: Установить Minikube

```
(venv) erohin@erohin-VirtualBox:~$ minikube version
minikube version: v1.25.2
commit: 362d5fdc0a3dbee389b3d3f1034e8023e72bd3a7

root@erohin-VirtualBox:/usr/bin# minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

root@erohin-VirtualBox:/usr/bin# kubectl get pods --namespace=kube-system
NAME                                        READY   STATUS    RESTARTS   AGE
coredns-64897985d-rbptj                     1/1     Running   0          113m
etcd-erohin-virtualbox                      1/1     Running   0          113m
kube-apiserver-erohin-virtualbox            1/1     Running   0          113m
kube-controller-manager-erohin-virtualbox   1/1     Running   0          113m
kube-proxy-26cfp                            1/1     Running   0          113m
kube-scheduler-erohin-virtualbox            1/1     Running   0          113m
storage-provisioner                         1/1     Running   0          113m
```

## Задача 2: Запуск Hello World

```
root@erohin-VirtualBox:~# kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   1/1     1            1           2m3s

root@erohin-VirtualBox:~# kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-6b89d599b9-smsf8   1/1     Running   0          2m29s

root@erohin-VirtualBox:~# kubectl get events
LAST SEEN   TYPE     REASON                    OBJECT                             MESSAGE
9m2s        Normal   NodeHasSufficientMemory   node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasSufficientMemory
9m2s        Normal   NodeHasNoDiskPressure     node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasNoDiskPressure
9m2s        Normal   NodeHasSufficientPID      node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasSufficientPID
8m47s       Normal   Starting                  node/erohin-virtualbox             Starting kubelet.
8m47s       Normal   NodeHasSufficientMemory   node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasSufficientMemory
8m47s       Normal   NodeHasNoDiskPressure     node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasNoDiskPressure
8m47s       Normal   NodeHasSufficientPID      node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeHasSufficientPID
8m47s       Normal   NodeAllocatableEnforced   node/erohin-virtualbox             Updated Node Allocatable limit across pods
8m37s       Normal   NodeReady                 node/erohin-virtualbox             Node erohin-virtualbox status is now: NodeReady
8m36s       Normal   RegisteredNode            node/erohin-virtualbox             Node erohin-virtualbox event: Registered Node erohin-virtualbox in Controller
8m33s       Normal   Starting                  node/erohin-virtualbox             
2m52s       Normal   Scheduled                 pod/hello-node-6b89d599b9-smsf8    Successfully assigned default/hello-node-6b89d599b9-smsf8 to erohin-virtualbox
2m52s       Normal   Pulling                   pod/hello-node-6b89d599b9-smsf8    Pulling image "k8s.gcr.io/echoserver:1.4"
2m10s       Normal   Pulled                    pod/hello-node-6b89d599b9-smsf8    Successfully pulled image "k8s.gcr.io/echoserver:1.4" in 41.807547372s
2m4s        Normal   Created                   pod/hello-node-6b89d599b9-smsf8    Created container echoserver
2m4s        Normal   Started                   pod/hello-node-6b89d599b9-smsf8    Started container echoserver
2m52s       Normal   SuccessfulCreate          replicaset/hello-node-6b89d599b9   Created pod: hello-node-6b89d599b9-smsf8
2m52s       Normal   ScalingReplicaSet         deployment/hello-node              Scaled up replica set hello-node-6b89d599b9 to 1

root@erohin-VirtualBox:~# kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Wed, 08 Jun 2022 17:21:33 CEST
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
        last-update: Wed, 08 Jun 2022 17:21:33 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/profiles/minikube/client.crt
    client-key: /root/.minikube/profiles/minikube/client.key

root@erohin-VirtualBox:~# kubectl get services
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   10.100.151.183   <pending>     8080:31342/TCP   27s
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP          11m

root@erohin-VirtualBox:~# curl $(minikube service hello-node --url)
CLIENT VALUES:
client_address=172.17.0.1
command=GET
real path=/
query=nil
request_version=1.1
request_uri=http://10.0.2.15:8080/

SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

HEADERS RECEIVED:
accept=*/*
host=10.0.2.15:31342
user-agent=curl/7.68.0
BODY:

root@erohin-VirtualBox:~# minikube addons list
|-----------------------------|----------|--------------|--------------------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |           MAINTAINER           |
|-----------------------------|----------|--------------|--------------------------------|
| ambassador                  | minikube | disabled     | third-party (ambassador)       |
| auto-pause                  | minikube | disabled     | google                         |
| csi-hostpath-driver         | minikube | disabled     | kubernetes                     |
| dashboard                   | minikube | enabled ✅   | kubernetes                     |
| default-storageclass        | minikube | enabled ✅   | kubernetes                     |
| efk                         | minikube | disabled     | third-party (elastic)          |
| freshpod                    | minikube | disabled     | google                         |
| gcp-auth                    | minikube | disabled     | google                         |
| gvisor                      | minikube | disabled     | google                         |
| helm-tiller                 | minikube | disabled     | third-party (helm)             |
| ingress                     | minikube | enabled ✅   | unknown (third-party)          |
| ingress-dns                 | minikube | disabled     | google                         |
| istio                       | minikube | disabled     | third-party (istio)            |
| istio-provisioner           | minikube | disabled     | third-party (istio)            |
| kong                        | minikube | disabled     | third-party (Kong HQ)          |
| kubevirt                    | minikube | disabled     | third-party (kubevirt)         |
| logviewer                   | minikube | disabled     | unknown (third-party)          |
| metallb                     | minikube | disabled     | third-party (metallb)          |
| metrics-server              | minikube | disabled     | kubernetes                     |
| nvidia-driver-installer     | minikube | disabled     | google                         |
| nvidia-gpu-device-plugin    | minikube | disabled     | third-party (nvidia)           |
| olm                         | minikube | disabled     | third-party (operator          |
|                             |          |              | framework)                     |
| pod-security-policy         | minikube | disabled     | unknown (third-party)          |
| portainer                   | minikube | disabled     | portainer.io                   |
| registry                    | minikube | disabled     | google                         |
| registry-aliases            | minikube | disabled     | unknown (third-party)          |
| registry-creds              | minikube | disabled     | third-party (upmc enterprises) |
| storage-provisioner         | minikube | enabled ✅   | google                         |
| storage-provisioner-gluster | minikube | disabled     | unknown (third-party)          |
| volumesnapshots             | minikube | disabled     | kubernetes                     |
|-----------------------------|----------|--------------|--------------------------------|
```

## Задача 3: Установить kubectl

```
root@erohin-VirtualBox:~# kubectl cluster-info
Kubernetes control plane is running at https://10.0.2.15:8443
CoreDNS is running at https://10.0.2.15:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.


root@erohin-VirtualBox:~# minikube start --vm-driver=none
---
Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

root@erohin-VirtualBox:~# kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Thu, 09 Jun 2022 09:05:39 CEST
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
        last-update: Thu, 09 Jun 2022 09:05:39 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.2
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/profiles/minikube/client.crt
    client-key: /root/.minikube/profiles/minikube/client.key


root@erohin-VirtualBox:~# kubectl port-forward hello-node-6b89d599b9-b49tm 31462:31462
Forwarding from [::1]:31462 -> 31462
```
