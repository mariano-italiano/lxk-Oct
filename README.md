# lxk-Oct

## Instalacja klastra

### Control plane

1. Przygotowujemy OS pod k8s (sysctl, modprobe, swap, repos, container runtime)
2. Instalacja kubectl, kubelet, kubeadm
3. Inicjalizacja klastra
4. Instalacja pluginu sieciowego
5. Skonfigurowanie `kubeconfig`

```sh
ssh -p 54321 student@altkom....
rm .ssh/known_hosts
ssh student@master
sudo -i
git clone <repo>
chmod +x deploy-k8s-ubuntu.sh
./deploy-k8s-ubuntu.sh

kubeadm init

kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

logout

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Worker nodes

1. Przygotowujemy OS pod k8s (sysctl, modprobe, swap, repos, container runtime)
2. Instalacja kubectl, kubelet, kubeadm
3. JOIN do klastra

Punkty 1-2 robi skrypt: `deploy-k8s-ubuntu.sh`.
