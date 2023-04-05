Voilà enfin le chapitre interessant, l'installation de notre cluster K3s.

Comme d'habitude, toutes les actions seront jouées depuis le node `control01`. 

## Installation du packages

### sur le master :
```bash 
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --disable servicelb --token dietpi --bind-address 192.168.1.200 --disable-cloud-controller --disable local-storage

kubectl get nodes
```

### sur les nodes :
```bash 
ansible nodes -b -m shell -a "curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.200:6443 K3S_TOKEN=dietpi sh -"

kubectl get nodes
```

## Gestion des Roles/Labels

Afin de s'y retrouver nous allons rajouter des roles et des labels à nos nodes.

```bash
## add Roles 
for i in $(seq -w 01 03); do 
  kubectl label nodes node$i kubernetes.io/role=worker
done
kubectl get nodes

## add labels
for i in $(seq -w 01 03); do 
  kubectl label nodes node$i node-type=worker
done
kubectl get nodes --show-labels
```

## Environment
Nous allons définir une variable d'environment, pour aider HELM a trouver les configurations Kubernetes.

```bash
ansible cube -b -m lineinfile -a "path='/etc/environment' line='KUBECONFIG=/etc/rancher/k3s/k3s.yaml'"
```