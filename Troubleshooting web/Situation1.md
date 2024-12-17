Clarembaux Robin 18/12/2024 L2

## Identification du problème

*Expliquez, du point de vue de l'utilisateur, ce qui ne fonctionne pas dans l'infrastructure. Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*

![1.png](img%2F1%2F1.png)  
![2.png](img%2F1%2F2.png)

Lorsqu'on tente d'accéder au blog via LINKS, cela redirige l'utilisateur vers le site principal (www) au lieu du blog.

## Collecte des symptômes

*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*

### Outil : dig

- **Nom** : dig
- **Machine et paramètres utilisés** : Machine du directeur, commande `dig blog.woodytoys.lab`
- **Output obtenu** :

  ![d1.png](img%2F1%2Fd1.png)

- **Interprétation** :

  Le DNS semble fonctionner correctement et redirige vers un serveur qui gère également le www. L'adresse IP du serveur web est également obtenue (comme visible avec Wireshark).

  ![d2.png](img%2F1%2Fd2.png)

### Outil : netstat

- **Nom** : netstat
- **Machine et paramètres utilisés** : Machine du serveur web (Apache), commande `ss -ptnlu`
- **Output obtenu** :

  ![d4.png](img%2F1%2Fd4.png)

- **Interprétation** :

  Il y a deux ports à l'écoute sur le serveur web, ce qui est intrigant.

### Outil : links

- **Nom** : links
- **Machine et paramètres utilisés** : Machine du directeur, commande `links http://blog.woodytoys.lab:8000`
- **Output obtenu** :

  ![d5.png](img%2F1%2Fd5.png)  
  ![e.png](img%2F1%2Fe.png)

- **Interprétation** :

  Lorsqu'on précise un port pour éviter d'utiliser le port 80 par défaut, on accède bien au blog.

## Description du problème

*Où se trouve l'erreur ?*  
Dans la configuration de l'environnement virtuel de la gestion Apache.

*Quel est le lien entre les symptômes et cette erreur ?*  
Étant donné que le blog fonctionnait sur le port 8000, l'utilisateur devait préciser le port 8000 pour accéder au blog.

## Proposition de solution

*Que faire pour régler le problème ?*  
Modifier le port dans la configuration des hôtes virtuels et éventuellement changer les ports afin que le serveur n'écoute plus sur le port 8000 inutilement. Utiliser le port 80 par défaut pour le HTTP.

![f2.png](img%2F1%2Ff2.png)

Notons qu'il fallait également redémarrer Apache.

*Comment valider que le problème est effectivement résolu ? Donnez les commandes à utiliser et prouvez que le bug est résolu en montrant l'output obtenu.*

Exécuter la commande : links http://blog.woodytoys.lab



(Sans spécifier de port, donc utilisation du port 80 par défaut)

**Output** :

![e.png](img%2F1%2Fe.png)

