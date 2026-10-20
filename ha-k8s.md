## HAProxy (Load Balancer)

```sh
global
    log /dev/log local0
    log /dev/log local1 notice
    daemon
    maxconn 4096

defaults
    log     global
    mode    tcp
    option  tcplog
    option  dontlognull
    timeout connect 5s
    timeout client  1h
    timeout server  1h
    timeout client-fin 30s
    timeout tunnel  1h

frontend k8s-api
    bind *:6443
    default_backend k8s-api-backend

backend k8s-api-backend
    option tcp-check
    balance roundrobin
    server cp1 192.168.1.141:6443 check
    server cp2 192.168.1.142:6443 check
    server cp3 192.168.1.143:6443 check

frontend stats
    bind *:8404
    mode http
    stats enable
    stats uri /
    stats refresh 10s
```

### Setup cp1
```sh
# .140 --> Load Balancer
# .141 --> IP cp1
kubeadm init --control-plane-endpoint 192.168.1.140:6443 --apiserver-advertise-address 192.168.1.141 --pod-network-cidr 10.244.0.0/16 --upload-certs
```


### Join cp2 & cp3
```sh
kubeadm join 192.168.1.140:6443 --token 67vhjo.cl77p9ff48qw8qjq     --discovery-token-ca-cert-hash sha256:592733782bb0a805516b680b59edb62164f14180dace6caf4a90a6f5b4138a76     --control-plane --certificate-key c6d20615d97357fd73e7f2f8ee00b0e19468df3868e860928def65b69197da93
```

### Etcd checks

```sh
kubectl -n kube-system exec etcd-cp1 -- etcdctl --endpoints=https://127.0.0.1:2379  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt  --key=/etc/kubernetes/pki/etcd/server.key member list -w table
kubectl -n kube-system exec etcd-cp1 -- etcdctl --endpoints=https://127.0.0.1:2379  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt  --key=/etc/kubernetes/pki/etcd/server.key endpoint health -w table
```
