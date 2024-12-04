

# DNS (Domain Name System)
### Système de gestion des noms de domaines



L'ICANN est un organisme qui gère la liste des *Top Level Domain* (TLD)
Il existe une TLD par pays
L'ICANN délègue la gestion de chaque TLD à un organisme (registry)
Chaque registry peut gérer comme bon lui semble l'attribution des noms de domaine sur sa TLD
Les registry ont un rôle technique
Chaque registry autorise des registrars à vendre des noms de domaine.
Les registrars ont un rôle commercial
Chaque pays possède un TLD qui correspond à son code pays ISO
On les appelle ccTLD (Country-Code TLD)
La gestion de ces TLD repose sur des serveurs racine (appelés DNS root server)

Le Service TCP/IP permet la correspondance un nom de domaine qualifié FQDN (Fully Qualified Domain Name) et une adresse IP

Domaine Inverse : Résolution d'une adresse IP en nom de domaine avec l'ajout d'un domaine spécial in-addr.arpa à la fin

### Zone primaire et zone secondaire

Transfert de zones entre serveur maître (primaire) et un autre serveur (secondaire), chacun ayant autorité sur la zone.
Vis-à-vis d'un client, l'un ou l'autre répond en fonction de la vitesse du réseaux

### Scénarios possible

- Serveur ca



### Serveur cache

But : Effectuer des requêtes DNS pour se rappeler de la réponse pour la prochaine requête

Avantages : 
- Réduction de la bande passante 
- Réduction du temps de latence


### Serveur primaire (maître)

But: Contenir des enregistrements DNS d'un nom de domaine enregistré

Un ensemble d'enregistrements DNS pour un nom de domaine est appelé une "zone"

Le nom de domaine peut être imaginaire, mais seulement sur un réseau local fermé, non connecté à internet.

### Serveur secondaire (esclave)

But: Contenir une copie des zones configurées sur le serveur maîre

Avantages : 
- Recommandé sur les réseaux
- asure la disponibilité

### Serveur stub

But: Idem que le serveur esclave, mais copie uniquement les données du serveur et pas les données de l'hôte


### Enregistrements DNS

But: mapper nom d'hôte / adresse IP et adresse IP / nom d'hôte

![[Pasted image 20241015093152.png]]




# DHCP

### Rôle du DHCP

Permet aux serveurs d'attribuer des adresses IP aux ordinateurs et autres périphériques reconnus comme client DHCP.
