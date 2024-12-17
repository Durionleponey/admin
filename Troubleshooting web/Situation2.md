## Identification du problème

*Expliquez, du point de vue de l'utilisateur, ce qui ne fonctionne pas dans l'infrastructure. Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*

Lorsqu'on tente d'accéder au site, on obtient une erreur **404 Not Found**. Cependant, en ajoutant `/index.html` à l'URL, le site est accessible.

![1.png](img%2F2%2F1.png)

## Collecte des symptômes

*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*

### Outil : Wireshark

- **Nom** : Wireshark
- **Machine et paramètres utilisés** : Utilisé entre la machine du directeur et le switch.
- **Output obtenu** :

  ![2.png](img%2F2%2F2.png)

- **Interprétation** :

  La communication avec le serveur web est établie correctement. Le serveur envoie même du HTML pour indiquer l'erreur 404. Cela suggère un problème possible de répertoire de travail incorrect, empêchant le serveur de trouver le fichier à envoyer.

### Outil : apache2ctl

- **Nom** : apache2ctl
- **Machine et paramètres utilisés** : Serveur web, commande `apache2ctl -S`
- **Output obtenu** :

  ![3.png](img%2F2%2F3.png)

- **Interprétation** :

  Le serveur est configuré avec des hôtes virtuels. Il est possible d'examiner leurs configurations pour identifier des erreurs.

### Outil : Logs du VirtualHost

- **Nom** : Logs du VirtualHost
- **Machine et paramètres utilisés** : Serveur web, commande `cat access.log`
- **Output obtenu** :

  ![4.png](img%2F2%2F4.png)

- **Interprétation** :

  Aucun élément inhabituel n'est relevé dans les logs d'accès. Après vérification des logs d'erreur (error.log) sans succès, il est nécessaire d'examiner la configuration des hôtes virtuels.

### Outil : Configuration du VirtualHost

- **Nom** : Configuration du VirtualHost `www.woodytoys.lab`
- **Machine et paramètres utilisés** : Serveur web, fichier `www.woodytoys-lab.conf`
- **Output obtenu** :

  ![5.png](img%2F2%2F5.png)

- **Interprétation** :

  Identification du `DocumentRoot` et de l'`IndexDirectory`.

### Outil : Vérification du Chemin des Fichiers

- **Nom** : Vérification du chemin des fichiers
- **Machine et paramètres utilisés** : Serveur web, commande `ls /var/www/html/www/`
- **Output obtenu** :

  ![6.png](img%2F2%2F6.png)

- **Interprétation** :

  Problème de nommage des fichiers : le fichier `index.html` existe, mais la configuration fait référence à `home.html`, ce qui cause l'erreur 404.

## Description du problème

*Où se trouve l'erreur ?*  
Dans le fichier de configuration du VirtualHost `www.woodytoys-lab.conf`.

*Quel est le lien entre les symptômes et cette erreur ?*  
L'erreur 404 indique que le serveur répond mais ne trouve pas la ressource demandée. Cela est dû à un mauvais chemin de fichier configuré dans le VirtualHost.

## Proposition de solution

*Que faire pour régler le problème ?*  
Deux solutions possibles :

1. **Renommer le `DirectoryIndex` en `index.html` dans le fichier de configuration :**

   ![7.png](img%2F2%2F7.png)

2. **Renommer le fichier `home.html` en `index.html` dans le répertoire :**

   Cependant, la première solution est préférable car elle respecte les conventions standards.

**Attention :** Ne pas oublier de redémarrer Apache après les modifications !

*Comment valider que le problème est effectivement résolu ? Donnez les commandes à utiliser et prouvez que le bug est résolu en montrant l'output obtenu.*

Exécuter la commande : links http://www.woodytoys.lab


**Output :**

![8.png](img%2F2%2F8.png)


