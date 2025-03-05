 


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


# Exemple packet tracer :

Part 1: Verify Local Connectivity and Test Access Control List
Step 1: Ping devices on the local network to verify connectivity.
a.     From the command prompt of PC1, ping PC2.
Oui ça ping

b.     From the command prompt of PC1, ping PC3.
Oui ça ping

Question:
Why were the pings successful?

Dans le routeur on retrouve

access-list 11 deny 192.168.10.0 0.0.0.255
access-list 11 permit any

Step 2: Ping devices on remote networks to test ACL functionality.
a.     From the command prompt of PC1, ping PC4.

Non ça ne marche pas

b.     From the command prompt of PC1, ping the DNS Server.

Non ça ne marche pas

Question:
Why did the pings fail? (Hint: Use simulation mode or view the router configurations to investigate.)

Ils n'ont pas les mêmes config et du coup ça ne marche pas

(network 10.10.1.0 0.0.0.3 area 0
network 10.10.1.4 0.0.0.3 area 0
network 192.168.10.0 0.0.0.255 area 10
network 192.168.11.0 0.0.0.255 area 11)


Part 2: Remove the ACL and Repeat the Test
Step 1: Use show commands to investigate the ACL configuration.
a.     Navigate to R1 CLI. Use the show run and show access-lists commands to view the currently configured ACLs. To quickly view the current ACLs, use show access-lists. Enter the show access-lists command, followed by a space and a question mark (?) to view the available options:

Open configuration window

R1# show access-lists ?

<1-199> ACL number

WORD ACL name

<cr>

If you know the ACL number or name, you can filter the show output further. However, R1 only has one ACL; therefore, the show access-lists command will suffice.

R1#show access-lists

Standard IP access list 11

10 deny 192.168.10.0 0.0.0.255

20 permit any

The first line of the ACL blocks any packets that originate in the 192.168.10.0/24 network, which includes Internet Control Message Protocol (ICMP) echoes (ping requests). The second line of the ACL allows all other ip traffic from any source to transverse the router.

b.     For an ACL to impact router operation, it must be applied to an interface in a specific direction. In this scenario, the ACL is used to filter traffic exiting an interface. Therefore, all traffic leaving the specified interface of R1 will be inspected against ACL 11.

Although you can view IP information with the show ip interface command, it may be more efficient in some situations to simply use the show run command. To obtain a complete list of interfaces that the ACL that may be applied to, and the list of all ACLs that are configured, use the following command:

R1# show run | include interface|access

interface GigabitEthernet0/0

interface GigabitEthernet0/1

interface Serial0/0/0

ip access-group 11 out

interface Serial0/0/1

interface Vlan1

access-list 11 deny 192.168.10.0 0.0.0.255

access-list 11 permit any

The second pipe symbol ‘|” creates an OR condition that matches ‘interface’ OR ‘access’. It is important that no spaces are included in the OR condition. Use one or both of these commands to find information about the ACL.

Question:
To which interface and in what direction is the ACL applied?

Step 2: Remove access list 11 from the configuration.
You can remove ACLs from the configuration by issuing the no access list [number of the ACL] command. The no access-list command when used without arguments deletes all ACLs configured on the router. The no access-list [number of the ACL] command removes only a specific ACL. Removing an ACL from a router does not remove the ACL from the interface. The command that applies the ACL to the interface must be removed separately.

a.     Under the Serial0/0/0 interface, remove access-list 11, which was previously applied to the interface as an outgoing filter:

R1(config)# interface s0/0/0

R1(config-if)# no ip access-group 11 out

b.     In global configuration mode, remove the ACL by entering the following command:

R1(config)# no access-list 11

c.     Verify that PC1 can now ping the DNS Server and PC4.

Oui ça ping.





---


