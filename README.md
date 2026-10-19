# lxk-Oct

## Instalacja klastra

### Control plane

1. Przygotowujemy OS pod k8s (sysctl, modprobe, swap, repos, container runtime)
2. Instalacja kubectl, kubelet, kubeadm
3. Inicjalizacja klastra
4. Instalacja pluginu sieciowego
5. Skonfigurowanie `kubeconfig`

### Worker nodes

1. Przygotowujemy OS pod k8s (sysctl, modprobe, swap, repos, container runtime)
2. Instalacja kubectl, kubelet, kubeadm
3. JOIN do klastra
