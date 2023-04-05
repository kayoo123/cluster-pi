## Raspberry-pi

Lorsque j'ai décidé de monter ce petit cluster, j'ai d'abord voulu choisir pour lui le meilleur (Rock64, RPI4 8Go RAM, etc...). 

Mais avec la pénurie des composants, c'est assez rare d'en trouver et les prix peuvent s'envoler très haut :money_with_wings:  .

Je me suis donc fait un choix de raison en selectionnant une version "en dessous", mais beaucoup plus accessible. La très populaire `Raspberry pi modele 3 B+`

Cette petite carte est le parfait compromis : 

- un SOC de 64-bit
- Un archi ARMv8
- Une frequence de 1.4Ghz

Elle est, de plus, accessible assez simplement sur internet (merci le-bon-coin), pour un prix très très abordable.
Mais n'hesité pas a demander autour de vous (collegues, amis ...) afin de completer votre collection.

Elle n'a malheureusement que peu de RAM (1GB LPDDR2), mais n'est-ce pas justement le but de faire un cluster: **Mutualiser les ressources** :chart_with_upwards_trend:

je commence ce projet avec 4x Raspberry, mais il aura l'avantage de grossir au fil du temps.


## Carte SD

Concernant le stockage du systeme, je me rabbat sur la solution des **cartes SD**. En effet, le boot sur USB n'est présente que sur mise-a-jour du firmware et ne semble bien fonctionner que depuis les modeles 4.
Le gain ne semble pas justifier cet usage.

Je reste donc a éplucher les bench pour identifier ce qui est nécessaire pour mon cluster, au minimum : 

- `C10` : definissant la vitesse d'ecriture **10Mo/s**.
- `A1` : définissant les IOPS (input/output operations per second) - **1500 en read et 500 en write**.
- `16GB` : pour heberger l'OS + quelques packages/images

Il n'est pas rare de trouver des lots carte SD à des prix très interessant. 
> Par exemple sur amazon, j'ai pu acheter 3x carte Kingston de `64GB C10 A1` pour une quinzaine d'euros. :tada:

Bien sur si vous trouvez des classe haute-vitesse (U3 - A2) pouvant allez jusqu'à 30Mo/s, c'est le top. 
Mais toujours dans l'esprit "cluster budget" je me rabbat sur les solutions les plus économes. 


## Stockage SSD

!!! TODO