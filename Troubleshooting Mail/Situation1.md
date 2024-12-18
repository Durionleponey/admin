# Clarembaux Robin  
**Date :** 18/12/2024  
**Niveau :** L2  

## Identification du problème
*Expliquez, du point de vue de l'utilisateur, ce qui ne marche pas dans l'infrastructure. Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*

Les employés peuvent consulter leurs emails via **mutt**, mais n'arrivent pas à en envoyer.  
J'ai utilisé simplement mutt connecté avec l'utilisateur **toto** sur le poste de la direction.

![1.png](1%2F1.png)

## Collecte des symptômes
*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*

### Wireshark
- **Utilisation :** Machine de l'atelier - switch
- **Paramètres :** Capture sur le réseau pertinent
- **Output :**
  ![2.png](1%2F2.png)
- **Déduction :** L'adresse de destination est rejetée avec un message "Access Denied".

### Telnet
- **Utilisation :** Machine de l'atelier
- **Commande :** `telnet mail.woodytoys.lab`
- **Output :**
  ![4.png](1%2F4.png)
- **Déduction :** Même problème que avec Wireshark, accès refusé.

### dig
- **Utilisation :** Serveur mail
- **Commande :** `dig mail.woodytoys.lab`
- **Output :**
  ![5.png](1%2F5.png)
- **Déduction :** Initialement pensé que le problème venait de la résolution des noms de domaine par le serveur mail. Après avoir modifié le fichier de résolveur pour utiliser le serveur DNS, le problème persiste avec "Access Denied". La résolution des noms n'est donc pas la cause.

### Consultation des logs
Je vous passe le détail des vérifications effectuées dans les logs, car elles n'ont rien apporté de plus.

### Vérification du fichier de configuration Postfix
- **Outil :** `cat main.cf`
- **Utilisation :** Serveur mail
- **Output :**
  ![10.png](1%2F10.png)
- **Déduction :** J'ai remarqué que l'adresse IP était `182.x.x.x` au lieu de `192.x.x.x` dans la directive `mynetworks`.

## Description du problème
- **Localisation de l'erreur :** Fichier `main.cf` de Postfix
- **Lien entre les symptômes et l'erreur :** Le réseau autorisé à accéder ou envoyer du serveur mail était incorrect. Par conséquent, l'accès à l'envoi des emails était refusé pour nos machines.

## Proposition de solution
- **Action à entreprendre :** Modifier l'adresse du réseau dans le fichier `main.cf` pour corriger l'erreur et relancer le service Postfix.
  
- **Validation de la résolution du problème :**
  - **Méthodes :** Utiliser `telnet` ou `mutt` pour essayer d'envoyer un email.
  - **Output attendu :**
    ![11.png](1%2F11.png)

