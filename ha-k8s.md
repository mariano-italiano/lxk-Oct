

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
