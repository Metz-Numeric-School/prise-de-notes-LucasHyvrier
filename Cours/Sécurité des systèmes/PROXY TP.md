


Créer une VM avec iso de OPNsense

Config : 

- Minum 2 coeur
- 4go de ram
- Do not use a network
- par defautl
- 20go minimum
- terminer
- ajouter deux carte réseau
- custom VMnet0 (nat)
- VMnet 10

Démarre ta VM


Dans la VM 

- Login : installer
- Password : opnsense

- French accent key
- Install ZFS
- Stripe - No Redundancy
- Sélectionné notre disque 
- YES
- Exit and reboot
- Déconnecter le lecteur cb de OPNsense


Si il n'y a pas d'IP qui s'affiche veuillez changer assignation des interface

Login : root
Password : opnsense

Modifier l'interface Lan :

- 2
- 1
- N
- Novelle IP : 192.168.1.254
- Mask de sous réseau : /24
- N
- Obtenir adresse ipv6 : N
- Entré
- Active le DHCP ? : y
- Donner l'étendu : 192.168.1.10 puis 192.168.1.20
- Changer le certificat : N
- signer certificat : N
- Pas de restore interface

ENSUITE UTILISER UNE VM WINDOWS

- Ajouter une carte réseau la même réseau c'est a dire VMnet 10 dans l'autre VM

PS : Normalement il y a internet sur la VM Windows 

Pour l'instant l'OPNsense c'est entre guillemet un routeur car il n'est pas encore config

Dans la barre de recherche de votre navigateur taper : 192.168.1.254 (ip de OPNsense)

- Username : root
- Password : opnsense
- Next

- Langage : Francais
- Serveur DSN principale : 8.8.8.8
- Next 

- Fuseau horaire : EU/Paris
- Next

- RIEN TOUCHER

- RIEN TOUCHER

- RIEN TOUCHER

- CHARGER


# Pare feu

Dans :
Système
puis
Firmware
puis
Mises a jour
Et 
Dans Status : 
Vérifier les mises à jour

Aller toute en bas de la page et cliquer sur mises à jour

On peut se reconnecter après la mises à jour

Créer un certificat ssl

Dans :
Système
puis
Certificat
puis 
Autorité

- Cliquer sur le +
	
	- Description : peut importe 
	- Pays : France
	- état : Grand-Est
	- Lieu : Metz
	- Organisation : MNS

Télécharger le comme un certificat
Renommer son extension en .crt
On le met en confiance en gros 
On peut créer une GPO pour le mettre pour plusieurs PC
Double clique sur la certification, il propose de l'installer = on l'installe sur l'ordinateur local
On le place dans le magasin autorité de certification racine de confiance
C'est bon la certification est bien importé 

On s'occupe du proxy maintenant

Dans :
Système 
puis
Firware
et 
dans Greffons

On recherche dans la barre de recherche le proxy "squid" et on l'installe
On redémarre OPSsense, dans le menu alimentation

On relance

On va dans squid Web Proxy puis dans forward proxy et on va signifier sur quel interface le proxy va être intégrer, on laisse le port de proxy par défaut.
Certification à utiliser = celui qu'on a utiliser précédemment.
Se rendre dans les paramètre généraux.
Activer le proxy, puis cliquer la petite flèche et aller dans les paramètre du cache local.
Cocher la case = stocker sur le proxy et non sur les caches des utilisateurs, il n'y auras pas de cache sur les navigateurs.
Si vous avez pas de serveur WSUS active le.

Cookie : fichier qui stock sur l'ordinateur (traçage) se qui te garde connecter, ou qu'il garde ton panier d'achat 

Puis on applique tous ça 

On va créer deux règles de filtrage dans le pare feu.
On va faire une déviation, on va faire que ça passe dans le proxy

On va se rendre dans le pare-feu, dans les règles. On va faire une règle dans le LAN.
On bloque tout le Traffic HTTP
Créer 2 nouvelles règles : ajouter 
Créer l'action bloquer le protocole TCP
Dans source : LAN net
HTTP
Description : blocage HTTP pour forcer le PROXY
Puis on sauvegarde
Et on duplique et modifie juste le HTTP en HTTPS dans la duplication
Les règles les plus hautes sont prioritaires, il faut les monter d'un niveau
Cocher les deux et cliquer la flèche du premier
Puis appliquer 'les changements'  

On a plus d'accès à internet car bloquer les Traffic HTTP et HTTPS
Mais on peut re accéder à internet manuellement
En tapant proxy dans la barre de recherche 
On va renseigner l'adresse IP du proxy : 192.168.1.254 sur le port 3129


Se rendre dans Squid Web -> Administration et activer le proxy transparent l'inspection SSL
Penser a désactiver le proxy manuel 
Faire pareil sur le proxy HTTP transparent -> sur le lien orange et appliquer les changements  
Tout est prérempli on doit juste sauvegarder
Petit I : ajouter la règle et on la sauvegarde et on "applique les changements"
Normalement on retrouve la connexion internet 

Dans proxy -> Administration -> Liste de contrôle d'accès 
Ajouter une liste "Black list", on doit taper une url
Description : black list
Puis on télécharge les ACLs 'télécharger ACL'
Editer la liste noir, sélectionner social network
Appliquer
Et télécharger ACLs


