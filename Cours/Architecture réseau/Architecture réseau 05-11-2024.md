Voici les commandes de configuration pour chaque appareil basé sur les informations de la capture d'écran. Les commandes ci-dessous sont pour Cisco IOS, généralement utilisé dans Packet Tracer.

### Routeur (R1)

Sur le routeur, nous allons configurer les sous-interfaces pour chaque VLAN avec l'encapsulation dot1Q (802.1Q) pour le routage inter-VLAN.

```plaintext
enable
configure terminal

interface Gig0/0/1
 description Trunk vers SW
 no shutdown

interface Gig0/0/1.10
 encapsulation dot1Q 10
 ip address 172.16.0.254 255.255.255.0  # Passerelle VLAN 10

interface Gig0/0/1.20
 encapsulation dot1Q 20
 ip address 172.16.1.62 255.255.255.192  # Passerelle VLAN 20

interface Gig0/0/1.40
 encapsulation dot1Q 40
 ip address 172.16.1.110 255.255.255.240  # Passerelle VLAN 40

interface Gig0/0/1.50
 encapsulation dot1Q 50
 ip address 172.16.1.126 255.255.255.240  # Passerelle VLAN 50

interface Gig0/0/1.60
 encapsulation dot1Q 60
 ip address 172.16.1.142 255.255.255.240  # Passerelle VLAN 60

exit
```
 
### Switch SW-1

#### Configuration des VLANs et des interfaces

```plaintext
enable
configure terminal

vlan 10
 name SALES

vlan 50
 name PRINTERS

interface range Fa0/1 - 18
 switchport mode access
 switchport access vlan 10

interface range Fa0/19 - 24
 switchport mode access
 switchport access vlan 50

interface Gig0/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,50

exit
```

### Switch SW-2

#### Configuration des VLANs et des interfaces

```plaintext
enable
configure terminal

vlan 20
 name R&D

vlan 50
 name PRINTERS

interface range Fa0/1 - 18
 switchport mode access
 switchport access vlan 20

interface range Fa0/19 - 24
 switchport mode access
 switchport access vlan 50

interface Gig0/0/1
 switchport mode trunk
 switchport trunk allowed vlan 20,50

exit
```

### Switch SW-3

#### Configuration des VLANs et des interfaces

```plaintext
enable
configure terminal

vlan 10
 name SALES

vlan 40
 name IT

vlan 50
 name PRINTERS

vlan 60
 name SERVERS

interface range Fa0/1 - 6
 switchport mode access
 switchport access vlan 40

interface range Fa0/7 - 12
 switchport mode access
 switchport access vlan 40

interface range Fa0/13 - 18
 switchport mode access
 switchport access vlan 50

interface range Fa0/19 - 24
 switchport mode access
 switchport access vlan 60

interface Gig0/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,40,50,60

exit
```

### Configuration supplémentaire pour le routage inter-VLAN (si requis)

Sur le routeur, assurez-vous d'ajouter le routage pour les VLANs si vous avez besoin de relier plusieurs VLANs à travers différents switches. Assurez-vous également que tous les switches sont connectés via des trunks pour permettre la communication inter-VLAN via le routeur.

N'hésitez pas à adapter ces configurations en fonction des spécificités de votre environnement ou des équipements disponibles.