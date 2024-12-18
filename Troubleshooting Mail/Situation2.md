# Clarembaux Robin  
**Date :** 18/12/2024  
**Classe :** L2  

## Identification du problème
*Expliquez, du point de vue de l'utilisateur, ce qui ne marche pas dans l'infrastructure. Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*

Les employés se plaignent de ne pas pouvoir consulter leurs emails via **mutt**.  
![1.png](2%2F1.png)

J'ai tenté un `dig` qui fonctionnait correctement :
![2.png](2%2F2.png)

J'ai également utilisé **Wireshark**, mais je n'ai rien trouvé de probant :
![3.png](2%2F3.png)

## Collecte des symptômes
*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*

### Telnet
- **Nom de l'outil :** Telnet
- **Utilisation :** Machine de l'atelier - Switch
- **Commande :** `telnet mail.woodytoys.lab 110`
- **Output :**
  ![7png.png](2%2F7png.png)
- **Déduction :** L'accès a échoué, ce qui indique un problème au niveau du serveur de messagerie.  
  **Réflexion :** Le fait que la connexion via Telnet sur le port 110 (POP3) échoue suggère que le service de réception des emails est injoignable, orientant vers un problème de configuration ou de chemin d'accès des boîtes mails.

### Logs de Dovecot
- **Nom de l'outil :** Logs de Dovecot
- **Utilisation :** Serveur mail
- **Commande :** `cat dovecot.log`
- **Output :**
  ![77.png](2%2F77.png)
- **Déduction :** Le chemin vers les boîtes mails était incorrect et n'existait pas, empêchant ainsi la connexion.  
  **Réflexion :** Une mauvaise configuration du chemin des boîtes mails dans `dovecot.conf` empêche Dovecot de localiser les emails des utilisateurs, rendant impossible la consultation des mails via des clients comme mutt.

## Description du problème 
- **Localisation de l'erreur :** Fichier de configuration `dovecot.conf`
- **Lien entre les symptômes et l'erreur :** Le chemin incorrect vers les boîtes mails dans la configuration de Dovecot empêchait le serveur de localiser et d'accéder aux emails des utilisateurs, ce qui rendait impossible la consultation des mails via mutt.

## Proposition de solution 
*Que faire pour régler le problème ?*

Modifier le chemin des boîtes mails dans le fichier `dovecot.conf` en remplaçant `etc/mail` par `var/mail`.

![777.png](2%2F777.png)

*Comment valider que le problème est effectivement résolu ? Donnez les commandes à utiliser et prouvez que le bug est résolu en montrant l'output obtenu.*

Utiliser **mutt** sur le poste du directeur pour envoyer et recevoir un email.

**Output attendu :**
![8.png](2%2F8.png)

**Validation :** Après modification, les tests doivent montrer que les emails peuvent être consultés et envoyés sans erreurs, confirmant ainsi que le problème de chemin d'accès a été résolu.

