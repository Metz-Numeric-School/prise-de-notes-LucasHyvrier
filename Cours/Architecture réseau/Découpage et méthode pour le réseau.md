


![[Pasted image 20241104091247.png]]


![[Pasted image 20241104093941.png]]
1. Câble Fibre  onde électromagnétique  Hub
2. Switch  adresse mac 48bits
3. Router  Dipv4  32bits  et  Dipv6  128bits
4. Firewall  Port  16bits
5. 



![[Pasted image 20241104095913.png]]


172.16.0.0


SALES 200 : 172.16.0.0 à 172.16.0.255 /24
R&D 50 : 172.16.1.0 à 172.16.1.63 /26
MGT 25 : 172.16.1.64 à 172.16.1.95 /27
IT 10 : 172.16.1.96 à 172.16.1.111 /28
PRINTERS 10 : 172.16.1.112 à 172.16.1.127 /28
SERVERS 10 : 172.16.1.128 à 172.16.1.143 /28

![[Pasted image 20241104112027.png]]

![[Pasted image 20241104114951.png]]
/12


Important : 
## Pour trouvé le nom du réseau tout mettre en binaire puis par rapport au /x conté jusqu'à atteindre le x puis mettre tout a zéro après le / et on trouve le nom du réseau



# Classe Adresse IP


A/8 :  10.x.x.x / 8
B/10 : 172.16.x.x / 12
C/24 : 192.168.x.x /16

192.168.0.0 -> 192.168.255.255 = 65536 adresses