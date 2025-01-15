



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








