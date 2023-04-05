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

## Test

Pour le test, on va simplement voir le retour du ping : 

```bash
ansible cube -m ping
```