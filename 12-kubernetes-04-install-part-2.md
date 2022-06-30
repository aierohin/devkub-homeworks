# Домашнее задание к занятию "12.4 Развертывание кластера на собственных серверах, лекция 2"
## Задание 1: Подготовить инвентарь kubespray
hosts.yaml
```
all:
  hosts:
    node1:
      ansible_host: 94.126.88.55
      ip: 94.126.88.55
      access_ip: 94.126.88.55
    node2:
      ansible_host: 94.126.88.56
      ip: 94.126.88.56
      access_ip: 94.126.88.56
    node3:
      ansible_host: 94.126.88.57
      ip: 94.126.88.57
      access_ip: 94.126.88.57
    node4:
      ansible_host: 94.126.88.58
      ip: 94.126.88.58
      access_ip: 94.126.88.58
    node5:
      ansible_host: 94.126.88.59
      ip: 94.126.88.59
      access_ip: 94.126.88.59
  children:
    kube_control_plane:
      hosts:
        node1:
    kube_node:
      hosts:
        node2:
        node3:
        node4:
        node5:
    etcd:
      hosts:
        node1:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}
```
k8s-cluster.yml
```
## Container runtime
## docker for docker, crio for cri-o and containerd for containerd.
## Default: containerd
container_manager: containerd
```
