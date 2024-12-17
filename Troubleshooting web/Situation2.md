## Identification du problème
*Expliquez, du point de vue de l'utilisateur, ce qui ne marche pas dans l'infrastructure.  Utilisez pour cela les outils utilisateurs (ping, links, mutt, ...) et montrez les outputs obtenus.*  

Quand on veut accéder au site on obtient une 404 not found (on peut y accéder si on rajoute un /index.html)  

![1.png](img%2F2%2F1.png)

## Collecte des symptômes
*Quels outils d'investigation sont intéressants dans cette situation ? Pour chacun d'eux :*  

*Nommez-le*  
Wireshark
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
Je l'utilise entre la machine du directeur et le switch
*Donnez un screenshot de l'output obtenu*  
![2.png](img%2F2%2F2.png)
*Indiquez ce que vous déduisez de cet output*  
On arrive bien à communiquer avec le serveur web, on reçoit même du html pour nous indiquer l'erreur 404  
Je peux en déduire que c'est certainement une question de mauvais working directory car c'est juste le serveur qui ne trouve pas le fichier à envoyer  


*Nommez-le*  
apache2ctl
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
serveur web - apache2ctl -S
*Donnez un screenshot de l'output obtenu* 
![3.png](img%2F2%2F3.png)
*Indiquez ce que vous déduisez de cet output*
Le serveur est configurer avec des virtual hosts je peux aller voir leurs configurations...


*Nommez-le*  
logs du virtualHost
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
web serveur - cat access.log
*Donnez un screenshot de l'output obtenu* 
![4.png](img%2F2%2F4.png)
*Indiquez ce que vous déduisez de cet output*
Pas grand chose de neuf, j'ai essayé aussi error.log rien non plus je vais donc aller voir la config de vhost  


*Nommez-le*  
config vhost www.woodytoys.lab
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
web serveur - www.woodytoys-lab.conf
*Donnez un screenshot de l'output obtenu* 
![5.png](img%2F2%2F5.png)
*Indiquez ce que vous déduisez de cet output*
Je retiens le DocumentRoot et le directory Index  



*Nommez-le*  
path fichier 
*Indiquez sur quelle machine (ou lien) vous l'utilisez, et avec quels paramètres*  
web serveur - ls /var/www/html/www/
*Donnez un screenshot de l'output obtenu*   
![6.png](img%2F2%2F6.png)
*Indiquez ce que vous déduisez de cet output* 
c'est bien un problème de nom de fichier ici index.html et dans le fichier de conf home.html  



## Description du problème 
*Où se trouve l'erreur?*  
dans le fichier de configuration du vhost www.woodytoys-lab.conf
*Quel est le lien entre les symptômes et cette erreur?* 
erreur 404 file not found, réponse du serveur mais ressource non trouvée, mauvais path  

## Proposition de solution   
*Que faire pour régler le problème?*  
2 solutions :  

1) renommer le directoryIndex en index.html dans le fichier de conf
![7.png](img%2F2%2F7.png)
OU
2) renommer le nom du fichier dans les var "home.html" mais c'est moins standard pas convention index.html est préférable

Attention : ne pas oublier de redémarrer apache !

*Comment valider que le problème est effectivement résolu? Donnez les commandes à utiliser et prouver que le bug est résolu en montrant l'output obtenu.*


links http://www.woodytoys.lab

![8.png](img%2F2%2F8.png)



