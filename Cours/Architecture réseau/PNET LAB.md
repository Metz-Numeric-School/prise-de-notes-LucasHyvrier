



Mettre 3 switches avec 5 bloque pour Ethernet
Renommé les switches : switch 1-3
prendre un switch et le rappeler L3

Mettre un routeur du nom R1

vPC en mettre 8 (dans c 8 pc on peut les mettre en Imprimante, serveur)
On peut changer les logos pour mieux se repérer 
Size permet de modifier la grosseur 


## Important :  

Chaque e0/0-e0/4 sont en lien trucks pour chaque switch 
Chaque e1/0-e1/4 sont pour les interfaces pour les vlan


Bien noté les découpages de réseaux. 

Exemple:

VLAN 10 : SALES  (200)
172.16.1.0/24
GW : 172.16.1.254

VLAN 20 : R&D (50)
172.16.2.0/26
255.255.255.192
GW : 172.16.2.62

VLAN 30 : MGT (25)
172.16.2.64/27
255.255.255.224
GW : 172.16.2.94

VLAN 40 : IT (10)
172.16.2.96/28
255.255.255.240
172.16.2.110

VLAN 50 : PRINTERS (10)
172.16.2.112/28
255.255.255.240
GW : 172.16.2.126

VLAN 60 : SERVERS (10)
172.16.2.128
255.255.255.240
GW : 172.16.2.133


## Important :

Créer des étiquettes pour chaque Vlan et mettre leur adresse IP en même temp, mettre des .1 pour les vPC et pour les imprimantes .113 à .115 

![[Pasted image 20250115110826.png]]

# Commande :

vPC-01 :

ip 172.16.1.1/24 172.16.1.254
? [(Pour voir toutes les commandes qu'on peut faire)
set pcname vPC-01 [(pour changer le nom mais dans le pc)
save

vPC-02 :

ip 172.16.2.1/26 172.16.2.62
set pcname vPC-02 
save

vPC-03 :

ip 172.16.2.65/27 172.16.2.94
set pcname vPC-03 
save


vPC-04 [LAPTOP] :

ip 172.16.2.97/28 172.16.2.110
set pcname vPC-04 
save


vPC-0-6 [SERVER] :

ip 172.16.2.129/28 172.16.2.142
set pcname vPC-06
save


vPRN-01 [IMPRIMANTE]  :

ip 172.16.2.113/28 172.16.2.126
set pcname vPRN-01 
save

vPRN-02  [IMPRIMANTE]  :

ip 172.16.2.114/28 172.16.2.126
set pcname vPRN-02 
save

vPRN-03  [IMPRIMANTE]  :

ip 172.16.2.114/28 172.16.2.126
set pcname vPRN-03 
save


# Important :

Déconnecter tout lien avec les switches, choisir 



SW-L3  [SWITCH leyer 4]  :

conf t 
vtp domain BSRC 2 (Pour donner un domaine)
vtp password MNS (Pour donner un MDP au moment de rentrer dans le domaine)
vlan 10
name SALES
vlan 20
name R&D
vlan 30
name MGT
vlan 40
name IT
vlan 50
name PRINTERS
vlan 60
name SERVERS
exit
show vlan br
conf t
int range E0/1-3
sw tr enc dot ([pour mettre le switch en mode trunk)
sw mode tr
exit
exit
wr

SW-01  [SWITCH ]  :

conf t
vtp mode client
vtp domain BSRC 2
vtp password MNS
int range E0/0-3
sw mo tr ([marche pas c normal])
sw tr enc dot
sw mo tr
exit
exit 
wr


SW-02  [SWITCH ]  :

conf t
vtp mode client
vtp domain BSRC 2
vtp password MNS
int range E0/0-3
sw tr enc dot
sw mo tr
exit
exit 
wr

SW-03  [SWITCH ]  :

conf t
vtp mode client
vtp domain BSRC 2
vtp password MNS
int range E0/0-3
sw tr enc dot
sw mo tr
exit
exit 
wr


# important :

### Relier tout ensemble 

SW-01  [SWITCH ]  :

Show vlan br
conf t
int range e1/0-3
sw mo ac
sw ac vlan 10
exit
int range e2/0-3
sw mo ac
sw ac vlan 50
exit
show int tr ([pour voir les interfaces en trunks])
"control z" 
wr

SW-02  [SWITCH ]  :

Show vlan br
conf t
int range e1/0-3
sw mo ac
sw ac vlan 10
exit
int range e2/0-3
sw mo ac
sw ac vlan 50
exit
show int tr 
"control z" 
wr

SW-03  [SWITCH ]  :

Show vlan br
conf t
int range e1/0-3
sw mo ac
sw ac vlan 30
exit
int range e2/0-3
sw mo ac
sw ac vlan 40
exit
int range e3/0-3
sw mo ac
sw ac vlan 50
exit
int range e4/0-3
sw mo ac
sw ac vlan 60
exit
show int tr 
"control z" 
wr




SW-L3  [SWITCH leyer 3 ]

conf t 
int vlan 10
ip add 172.16.1.254 255.255.255.0
no shut
exit
int vlan 20
ip add 172.16.2.62 255.255.255.192
no shut
exit
int vlan 30
ip add 172.16.2.94 255.255.255.224
no shut
exit
int vlan 40
ip add 172.16.2.110 255.255.255.240
no shut
exit
int vlan 50
ip add 172.16.2.126 255.255.255.240
no shut
exit
int vlan 60
ip add 172.16.2.142 255.255.255.240
no shut
exit
"controle z"
sh ip int br ([pour voir si elles sont up et capable de ping la passerelle)

# Test sur vPC

vPC-01  
ping 172.16.2.254

vPC-02  
ping 172.16.2.62

## Normalement sa marche pas c normal

ping 172.16.2.113 [(ping imprimante car par default il y a "ip routing" dans le layer 3 mais normalement ça ne marche pas)

SW-L3  [SWITCH leyer 3 ]

conf t ip routing [(la commande qui déclenche le routage inter Vlan)]

# Éteindre les machines c mieux une fois qu'on l'utilise plus  


# Test sur vPC

vPC-01  
ping 172.16.2.254

vPC-02  
ping 172.16.2.62














