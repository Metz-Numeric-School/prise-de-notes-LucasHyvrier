 


Standard :
- Numérotée : 1-99 et 1300-1999
- Filtrage : IPv4 Source

**Les ACLs standard se position toujours a l'interface la plus proche.**
On peut aussi les configurer pour la mettre en sorti et en entrée par interface.


Etendue :
- Numérotée : 100-199 et 2000-2699
- Nommée
- Filtrage : IPv4 Source et Destination et IPv6 Source et Destination
- Ports
- Protocoles
- @MAC (Option)

Les étendue c plutôt pour bloqué des interfaces proche de la source. Exemple :

Deny @IPv4-1 -> @IPv4-2 in

Image ci-dessous :

![[Pasted image 20250305091948.png]]


Mots clés :
- Any
- Host
- *Established (ACL Etendue)*


# Gestion des ACLs

### **Wild card** 

Il faut mettre tout a 1 puis soustraire le masque. Exemple : 

	255 . 255 . 255 . 255
	255 . 255 . 255 .  0   / 24
	----------------------;
	  0  .  0  .  0 .  255


	255 . 255 . 255 . 255
	255 . 255 . 255 . 192   /26
	-----------------------;
	0   .  0  .  0  .  64


# Commande ACLs


access-list

ip access-list


Vérification : 

show access-list
show ip interface






