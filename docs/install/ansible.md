## Inventory

Toujours sur notre node `control01`, nous allons installer l'outil **ansible** nous permettant de lancer des actions sur l'ensemble des noeuds du cluster (y compris soit-même).

```bash
## Installation 
apt install -y ansible

## Creation de l'inventaire
mkdir /etc/ansible

cat <<EOF>/etc/ansible/hosts
[control]
control01  ansible_connection=local

[nodes]
node01  ansible_connection=ssh
node02  ansible_connection=ssh
node03  ansible_connection=ssh

[cube:children]
control
nodes
EOF
```



Pour le test, on va simplement voir le retour du ping : 

```bash
ansible cube -m ping
```

## Installation IPtables

Et nous voilà partie, on va pouvoir finaliser notre systeme en installant le package `iptables` qui sera nécessaire pour l'utilisation du **K3s** :

```
ansible cube -m apt -a "name=iptables state=present" --become
ansible nodes -b -m shell -a "reboot"
```