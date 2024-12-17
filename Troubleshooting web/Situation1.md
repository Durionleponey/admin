## Identification du problème
*Expliquez, du point de vue de l'utilisateur, ce qui ne marche pas dans l'infrastructure.  Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*
![1.png](img%2F1%2F1.png)
![2.png](img%2F1%2F2.png)
Quand on veut faire un LINKS vers le blog cela renvoi l'user vers le www et non vers le blog

## Collecte des symptômes
*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*

*Nommez-le*
*dig*
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
machine du directeur dig blog.woodytoys.lab
*Donnez un screenshot de l'output obtenu*   
![d1.png](img%2F1%2Fd1.png)
*Indiquez ce que vous déduisez de cet output*  
Je déduis que le dns a l'air de fonctionner et que on me redirige sur un serveur qui gère aussi le www
J'ai aussi l'ip du serveur web (on pouvait voir ca aussi avec wireshark)![d2.png](img%2F1%2Fd2.png)


*Nommez-le*  
netstat
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
machine www apache commande: ss -ptnlu
*Donnez un screenshot de l'output obtenu*   
![d4.png](img%2F1%2Fd4.png)
*Indiquez ce que vous déduisez de cet output*  
Il y a 2 ports à l'écoute sur le serveur web, ça m'intrigue...

*Nommez-le*    
links
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
machine directeur commande: links http://blog.woodytoys.lab:8000
*Donnez un screenshot de l'output obtenu*    
![d5.png](img%2F1%2Fd5.png)![e.png](img%2F1%2Fe.png)
*Indiquez ce que vous déduisez de cet output*    
Quand on précise un port pour éviter de prendre le port 80 par default on accède bien au blog




## Description du problème 
*Où se trouve l'erreur?*  
Dans la configuration de l'environnement virtuel de la gestion apache
*Quel est le lien entre les symptômes et cette erreur?*  
Vu que blog vivait sur le port 8000 l'utilisateur devait préciser le port 8000 pour accéder au blog!
## Proposition de solution 
*Que faire pour régler le problème?*  
changer le port dans la config hotes virtuels & éventuellemnent changer les ports comme
ça le serveur n'écoute plus le 8000 pour rien. Donc mettre port 80 à la place le port par default pour le http
![f2.png](img%2F1%2Ff2.png)
Notons qu'il fallait aussi redémarrer apache  
*Comment valider que le problème est effectivement résolu? Donnez les commandes à utiliser et prouver que le bug est résolu en montrant l'output obtenu.*  

links http://blog.woodytoys.lab (sans précisions de port du coup port 80 par défault)  


Output:
![e.png](img%2F1%2Fe.png)