## DNS

Afin de discuter simplement avec le nom d'equipement (et non par adressage IP), nous allons renseigner notre fichier d'**hosts** sur notre noeud **control01**.

!!! Note
    Bien sur, si vous utilisez un serveur DNS comme moi, il est plus simple de l'utiliser afin de forcer la rësolution DNS sur l'ensemble des péripheriques de votre reseau.

Connectez-vous sur le node : `control01` et éditer le fichier `/etc/hosts`
```bash
127.0.0.1 localhost

192.168.1.200 control01 control01.local
192.168.1.201 node01 node01.local
192.168.1.202 node02 node02.local
192.168.1.203 node03 node03.local
```

## Clé SSH

Ensuite, nous allons faire en sorte que le node `control01` est accès aux autres nodes, via clé SSH.
Comme ca, nous lui ferons confiance et nous n'aurons plus a renseigner un mot de passe (ce qui sera utile pour la suite avec ansible).

```bash
ssh-keygen -t rsa -q -N "" -f ~/.ssh/cube
ssh-copy-id -i ~/.ssh/cube.pub root@node01
ssh-copy-id -i ~/.ssh/cube.pub root@node02
ssh-copy-id -i ~/.ssh/cube.pub root@node03
```

