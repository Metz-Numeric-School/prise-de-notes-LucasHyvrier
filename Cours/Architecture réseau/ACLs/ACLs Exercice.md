
Exercice : 4.1.4 Packet Tracer - ACL Demonstration

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


# Part 2: Remove the ACL and Repeat the Test
### Step 1: Use show commands to investigate the ACL configuration.

a.     Navigate to R1 CLI. Use the show run and show access-lists commands to view the currently configured ACLs. To quickly view the current ACLs, use show access-lists. Enter the show access-lists command, followed by a space and a question mark (?) to view the available options:

Open configuration window

R1# show access-lists ?

<1-199> ACL number

WORD ACL name

cr

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





## Exercice : 5.1.8 Packet Tracer - Configure Numbered Standard IPv4 ACLs



Objectives
### Part 1: Plan an ACL Implementation

### Part 2: Configure, Apply, and Verify a Standard ACL

Background / Scenario
Standard access control lists (ACLs) are router configuration scripts that control whether a router permits or denies packets based on the source address. This activity focuses on defining filtering criteria, configuring standard ACLs, applying ACLs to router interfaces, and verifying and testing the ACL implementation. The routers are already configured, including IP addresses and Enhanced Interior Gateway Routing Protocol (EIGRP) routing.

Instructions
### Part 1: Plan an ACL Implementation

Step 1: Investigate the current network configuration.
Before applying any ACLs to a network, it is important to confirm that you have full connectivity. Verify that the network has full connectivity by choosing a PC and pinging other devices on the network. You should be able to successfully ping every device.

Step 2: Evaluate two network policies and plan ACL implementations.
a.     The following network policies are implemented on R2:

·         The 192.168.11.0/24 network is not allowed access to the WebServer on the 192.168.20.0/24 network.

·         All other access is permitted.

To restrict access from the 192.168.11.0/24 network to the WebServer at 192.168.20.254 without interfering with other traffic, an ACL must be created on R2. The access list must be placed on the outbound interface to the WebServer. A second rule must be created on R2 to permit all other traffic.

b.     The following network policies are implemented on R3:

·         The 192.168.10.0/24 network is not allowed to communicate with the 192.168.30.0/24 network.

·         All other access is permitted.

To restrict access from the 192.168.10.0/24 network to the 192.168.30/24 network without interfering with other traffic, an access list will need to be created on R3. The ACL must be placed on the outbound interface to PC3. A second rule must be created on R3 to permit all other traffic.

Part 2: Configure, Apply, and Verify a Standard ACL
Step 1: Configure and apply a numbered standard ACL on R2.
a.     Create an ACL using the number 1 on R2 with a statement that denies access to the 192.168.20.0/24 network from the 192.168.11.0/24 network.

Open configuration window

R2(config)# access-list 1 deny 192.168.11.0 0.0.0.255

b.     By default, an access list denies all traffic that does not match any rules. To permit all other traffic, configure the following statement:

R2(config)# access-list 1 permit any

c.     Before applying an access list to an interface to filter traffic, it is a best practice to review the contents of the access list, in order to verify that it will filter traffic as expected.

R2# show access-lists

Standard IP access list 1

10 deny 192.168.11.0 0.0.0.255

20 permit any

d.     For the ACL to actually filter traffic, it must be applied to some router operation. Apply the ACL by placing it for outbound traffic on the GigabitEthernet 0/0 interface. Note: In an actual operational network, it is not a good practice to apply an untested access list to an active interface.

R2(config)# interface GigabitEthernet0/0

R2(config-if)# ip access-group 1 out

Step 2: Configure and apply a numbered standard ACL on R3.
a.     Create an ACL using the number 1 on R3 with a statement that denies access to the 192.168.30.0/24 network from the PC1 (192.168.10.0/24) network.

R3(config)# access-list 1 deny 192.168.10.0 0.0.0.255

b.     By default, an ACL denies all traffic that does not match any rules. To permit all other traffic, create a second rule for ACL 1.

R3(config)# access-list 1 permit any

c.     Verify that the access list is configured correctly.

R3# show access-lists

Standard IP access list 1

10 deny 192.168.10.0 0.0.0.255

20 permit any

d.     Apply the ACL by placing it for outbound traffic on the GigabitEthernet 0/0 interface.

R3(config)# interface GigabitEthernet0/0

R3(config-if)# ip access-group 1 out

Step 3: Verify ACL configuration and functionality.
a.     Enter the show run or show ip interface gigabitethernet 0/0 command to verify the ACL placements.

b.     With the two ACLs in place, network traffic is restricted according to the policies detailed in Part 1. Use the following tests to verify the ACL implementations:

·         A ping from 192.168.10.10 to 192.168.11.10 succeeds.

·         A ping from 192.168.10.10 to 192.168.20.254 succeeds.

·         A ping from 192.168.11.10 to 192.168.20.254 fails.

·         A ping from 192.168.10.10 to 192.168.30.10 fails.

·         A ping from 192.168.11.10 to 192.168.30.10 succeeds.

·         A ping from 192.168.30.10 to 192.168.20.254 succeeds.

#### Tout les ping marche bien


c.     Issue the show access-lists command again on routers R2 and R3. You should see output that indicates the number of packets that have matched each line of the access list. Note: The number of matches shown for your routers may be different, due to the number of pings that are sent and received.

R2# show access-lists

Standard IP access list 1

10 deny 192.168.11.0 0.0.0.255 (4 match(es))

20 permit any (8 match(es))

 

R3# show access-lists

Standard IP access list 1

10 deny 192.168.10.0 0.0.0.255 (4 match(es))

20 permit any (8 match(es))

![[Pasted image 20250305110340.png]]


## Exercice : 5.1.9 Packet Tracer - Configure Named Standard IPv4 ACLs


Objectives
Part 1: Configure and Apply a Named Standard ACL

Part 2: Verify the ACL Implementation

Background / Scenario
The senior network administrator has asked you to create a standard named ACL to prevent access to a file server. The file server contains the data base for the web applications. Only the Web Manager workstation PC1 and the Web Server need to access the File Server. All other traffic to the File Server should be denied.

Instructions
### Part 1: Configure and Apply a Named Standard ACL
Step 1: Verify connectivity before the ACL is configured and applied.
All three workstations should be able to ping both the Web Server and File Server.

Step 2: Configure a named standard ACL.
Open configuration window

a.     Configure the following named ACL on R1.

R1(config)# ip access-list standard File_Server_Restrictions

R1(config-std-nacl)# permit host 192.168.20.4

R1(config-std-nacl)# permit host 192.168.100.100

R1(config-std-nacl)# deny any

Note: For scoring purposes, the ACL name is case-sensitive, and the statements must be in the same order as shown.

b.     Use the show access-lists command to verify the contents of the access list before applying it to an interface. Make sure you have not mistyped any IP addresses and that the statements are in the correct order.

R1# show access-lists

Standard IP access list File_Server_Restrictions

10 permit host 192.168.20.4

20 permit host 192.168.100.100

30 deny any

Step 3: Apply the named ACL.
a.     Apply the ACL outbound on the Fast Ethernet 0/1 interface.

Note: In an actual operational network, applying an access list to an active interface is not a good practice and should be avoided if possible.

R1(config-if)# ip access-group File_Server_Restrictions out

b.     Save the configuration.

Close configuration window

### Part 2: Verify the ACL Implementation
Step 1: Verify the ACL configuration and application to the interface.
Open configuration window

Use the show access-lists command to verify the ACL configuration. Use the show run or show ip interface fastethernet 0/1 command to verify that the ACL is applied correctly to the interface.

Step 2: Verify that the ACL is working properly.
All three workstations should be able to ping the Web Server, but only PC1 and the Web Server should be able to ping the File Server. Repeat the show access-lists command to see the number of packets that matched each statement.


![[Pasted image 20250305105812.png]]


## Exercice : 5.2.7 Packet Tracer - Configure and Modify Standard IPv4 ACLs


Objectives
### Part 1: Verify Connectivity

### Part 2: Configure and Verify Standard Numbered and Named ACLs

### Part 3: Modify a Standard ACL

Background / Scenario
Network security and traffic flow control are important issues when designing and managing IP networks. The ability to configure proper rules to filter packets, based on established security policies, is a valuable skill.

In this lab, you will set up filtering rules for two business locations that are represented by R1 and R3. Management has established some access policies between the LANs located at R1 and R3, which you must implement. The Edge router sitting between R1 and R3 has been provided by the ISP will not have any ACLs placed on it. You would not be allowed any administrative access to the Edge router because you can only control and manage your own equipment.

Instructions
### Part 1: Verify Connectivity

In Part 1, you verify connectivity between devices.

Note: It is very important to test whether connectivity is working before you configure and apply access lists. You want to be sure that your network is properly functioning before you start to filter traffic.

Questions:
From PC-A, ping PC-C and PC-D. Were your pings successful? Ya ça ping

From R1, ping PC-C and PC-D. Were your pings successful? Ya ça ping

From PC-C, ping PC-A and PC-B. Were your pings successful? Ya ça ping 

From R3, ping PC-A and PC-B. Were your pings successful? Ya ça ping

Can all of the PCs ping the server at 209.165.200.254? Ya ça marche

### Part 2: Configure and Verify Standard Numbered and Named ACLs

Step 1: Configure a numbered standard ACL.
Standard ACLs filter traffic based on the source IP address only. A typical best practice for standard ACLs is to configure and apply the ACL as close to the destination as possible. For the first access list in this activity, create a standard numbered ACL that allows traffic from all hosts on the 192.168.10.0/24 network and all hosts on the 192.168.20.0/24 network to access all hosts on the 192.168.30.0/24 network. The security policy also states that an explicit deny any access control entry (ACE), also referred to as an ACL statement, should be present at the end of all ACLs.

Questions:
What wildcard mask would you use to allow all hosts on the 192.168.10.0/24 network to access the 192.168.30.0/24 network?  255.255.255.0

Following Cisco’s recommended best practices, on which router would you place this ACL?
R1

On which interface would you place this ACL? In what direction would you apply it?

interface Serial0/1/0 sur les deux directions

a.     Configure the ACL on R3. Use 1 for the access list number.

Open configuration window

R3(config)# access-list 1 remark Allow R1 LANs Access

R3(config)# access-list 1 permit 192.168.10.0 0.0.0.255

R3(config)# access-list 1 permit 192.168.20.0 0.0.0.255

R3(config)# access-list 1 deny any

b.     Apply the ACL to the appropriate interface in the proper direction.

R3(config)# interface g0/0/0

R3(config-if)# ip access-group 1 out

c.     Verify a numbered ACL.

The use of various show commands can help you to verify both the syntax and placement of your ACLs in your router.

Questions:
To see access list 1 in its entirety with all ACEs, which command would you use?

show ip interface g0/0/0

What command would you use to see where the access list was applied and in what direction? 

show access-lists 1

1)    On R3, issue the show access-lists 1 command.

R3# show access-list 1

Standard IP access list 1

permit 192.168.10.0, wildcard bits 0.0.0.255

permit 192.168.20.0, wildcard bits 0.0.0.255

deny any

1)    On R3, issue the show ip interface g0/0/0 command.

R3# show ip interface g0/0/0

GigabitEthernet0/0/0 is up, line protocol is up (connected)

Internet address is 192.168.30.1/24

Broadcast address is 255.255.255.255

Address determined by setup command

MTU is 1500 bytes

Helper address is not set

Directed broadcast forwarding is disabled

Outgoing access list is 1

Inbound access list is not set

Output omitted Questions:

1)    Test the ACL to see if it allows traffic from the 192.168.10.0/24 network to access the 192.168.30.0/24 network.

From the PC-A command prompt, ping the PC-C IP address. Were the pings successful? Oui ça marche

1)    Test the ACL to see if it allows traffic from the 192.168.20.0/24 network access to the 192.168.30.0/24 network.

From the PC-B command prompt, ping the PC-C IP address. Were the pings successful?

Oui ça marche

1)    Should pings from PC-D to PC-C be successful? Ping from PC-D to PC-C to verify your answer.

Oui ça marche

d.     From the R1 prompt, ping PC-C’s IP address again.

Non ça marche pas  

R1# ping 192.168.30.3

Question:
Was the ping successful? Explain. ça ne marche pas car R1 ne connait pas le réseau 192.168.30.0

e.     Issue the show access-lists 1 command again. Note that the command output displays information for the number of times each ACE was matched by traffic that reached interface Gigabit Ethernet 0/0/0.

R3# show access-lists 1

Standard IP access list 1

permit 192.168.10.0 0.0.0.255 (4 match(es))

permit 192.168.20.0 0.0.0.255 (4 match(es))

deny any (4 match(es))

Step 2: Configure a named standard ACL.
Create a named standard ACL that conforms to the following policy: allow traffic from all hosts on the 192.168.40.0/24 network access to all hosts on the 192.168.10.0/24 network. Also, only allow host PC-C access to the 192.168.10.0/24 network. The name of this access list should be called BRANCH-OFFICE-POLICY.

Questions:
Following Cisco’s recommended best practices, on which router would you place this ACL?

On which interface would you place this ACL? In what direction would you apply it?

a.     Create the standard named ACL BRANCH-OFFICE-POLICY on R1.

R1(config)# ip access-list standard BRANCH-OFFICE-POLICY

R1(config-std-nacl)# permit host 192.168.30.3

R1(config-std-nacl)# permit 192.168.40.0 0.0.0.255

R1(config-std-nacl)# end

R1#

*Feb 15 15:56:55.707: %SYS-5-CONFIG_I: Configured from console by console

Question:
Look at the first ACE in the access list. What is another way to write this?

b.     Apply the ACL to the appropriate interface in the proper direction.

R1# config t

R1(config)# interface g0/0/0

R1(config-if)# ip access-group BRANCH-OFFICE-POLICY out

c.     Verify a named ACL.

1)    On R1, issue the show access-lists command.

R1# show access-lists

Standard IP access list BRANCH-OFFICE-POLICY

10 permit host 192.168.30.3

20 permit 192.168.40.0 0.0.0.255

Question:
Is there any difference between this ACL on R1 and the ACL on R3? If so, what is it?

1)    On R1, issue the show ip interface g0/0/0 command to verify that the ACL is configured on the interface.

R1# show ip interface g0/0/0

GigabitEthernet0/0/0 is up, line protocol is up (connected)

Internet address is 192.168.10.1/24

Broadcast address is 255.255.255.255

Address determined by setup command

MTU is 1500 bytes

Helper address is not set

Directed broadcast forwarding is disabled

Outgoing access list is BRANCH-OFFICE-POLICY

Inbound access list is not set

Output omitted>Question:

Test the ACL. From the command prompt on PC-C, ping the IP address of PC-A. Were the pings successful?

1)    Test the ACL to ensure that only the PC-C host is allowed access to the 192.168.10.0/24 network. You must do an extended ping and use the G0/0/0 address on R3 as your source. Ping PC-A’s IP address.

R3# ping

Protocol [ip]:

Target IP address: 192.168.10.3

Repeat count [5]:

Datagram size [100]:

Timeout in seconds [2]:

Extended commands [n]: y

Source address or interface: 192.168.30.1

Type of service [0]:

Set DF bit in IP header? [no]:

Validate reply data? [no]:

Data pattern [0xABCD]:

Loose, Strict, Record, Timestamp, Verbose[none]:

Sweep range of sizes [n]:

Type escape sequence to abort.

Sending 5, 100-byte ICMP Echos to 192.168.10.3, timeout is 2 seconds:

Packet sent with a source address of 192.168.30.1

U.U.U

Question:
Were the pings successful? Nan ça marche pas 

1)    Test the ACL to see if it allows traffic from the 192.168.40.0/24 network access to the 192.168.10.0/24 network. From the PC-D command prompt, ping the PC-A IP address.

Question:
Were the pings successful?  Oui ça marche

Close configuration window

### Part 3: Modify a Standard ACL

It is common in business for security policies to change. For this reason, ACLs may need to be modified. In Part 3, you will change one of the ACLs you configured previously to match a new management policy that is being put in place.

Attempt to ping the server at 209.165.200.254 from PC-A. Notice that the ping is not successful. The ACL on R1 is blocking internet traffic from returning to PC-A. This is because the source address in the packets that are returned is not in the range of permitted addresses.

Management has decided that traffic that is returning from the 209.165.200.224/27 network should be allowed full access to the 192.168.10.0/24 network. Management also wants ACLs on all routers to follow consistent rules. A deny any ACE should be placed at the end of all ACLs. You must modify the BRANCH-OFFICE-POLICY ACL.

You will add two additional lines to this ACL. There are two ways you could do this:

OPTION 1: Issue a no ip access-list standard BRANCH-OFFICE-POLICY command in global configuration mode. This would remove the ACL from the router. Depending upon the router IOS, one of the following scenarios would occur: all filtering of packets would be cancelled, and all packets would be allowed through the router; or, because you did not remove the ip access-group command from the G0/1 interface, filtering is still in place. Regardless, when the ACL is gone, you could retype the whole ACL, or cut and paste it in from a text editor.

OPTION 2: You can modify ACLs in place by adding or deleting specific lines within the ACL itself. This can come in handy, especially with ACLs that are long. The retyping of the whole ACL or cutting and pasting can easily lead to errors. Modifying specific lines within the ACL is easily accomplished.

For this activity, use Option 2.

Step 1: Modify a named standard ACL.
a.     From R1, issue the show access-lists command.

Open configuration window

R1# show access-lists

Standard IP access list BRANCH-OFFICE-POLICY

10 permit 192.168.30.3 (8 matches)

20 permit 192.168.40.0 0.0.0.255 (5 matches)

b.     Add two additional lines at the end of the ACL. From global config mode, modify the ACL, BRANCH-OFFICE-POLICY.

R1#(config)# ip access-list standard BRANCH-OFFICE-POLICY

R1(config-std-nacl)# 30 permit 209.165.200.224 0.0.0.31

R1(config-std-nacl)# 40 deny any

R1(config-std-nacl)# end

c.     Verify the ACL.

1)    On R1, issue the show access-lists command.

R1# show access-lists

Standard IP access list BRANCH-OFFICE-POLICY

10 permit 192.168.30.3 (8 matches)

20 permit 192.168.40.0, wildcard bits 0.0.0.255 (5 matches)

30 permit 209.165.200.224, wildcard bits 0.0.0.31

40 deny any

Question:
Do you have to apply the BRANCH-OFFICE-POLICY to the G0/1 interface on R1?

Oui 

1)    Test the ACL to see if it allows traffic from the 209.165.200.224/27 network access to return to the 192.168.10.0/24 network. From PC-A, ping the server at 209.165.200.254.

Question:
Were the pings successful? Oui ça marche bien

Close configuration window

Reflection Questions
1.     As you can see, standard ACLs are very powerful and work quite well. Why would you ever have the need for using extended ACLs?

Les ACL étendues permettent un filtrage plus précis que les ACL standard en tenant compte de l’adresse source et destination.

2.     More typing is typically required when using a named ACL as opposed to a numbered ACL. Why would you choose named ACLs over numbered?

Les ACL nommées sont plus lisibles car elles utilisent des noms explicites au lieu de simples numéros. Elles sont plus flexibles car on peut modifier, ajouter ou supprimer des règles sans recréer toute l'ACL. Elles permettent une meilleure documentation avec des commentaires pour expliquer les règles. Leur gestion est plus organisée et évolutive, surtout dans les réseaux complexes. Enfin, elles facilitent le dépannage et la maintenance en rendant les configurations plus compréhensibles.
![[Pasted image 20250305114833.png]]




## Exercice : 5.4.12 Packet Tracer - Configure Extended IPv4 ACLs - Scenario 1

### **Partie 1 : Configurer, Appliquer et Vérifier une ACL Numérotée Étendue**

#### **Étape 1 : Configurer une ACL pour permettre FTP et ICMP depuis le réseau de PC1**

1. **Quel est le premier numéro valide pour une ACL étendue ?**  
    → **100** (les ACLs étendues vont de **100 à 199**).
    
2. **Pourquoi FTP n’apparaît-il pas dans la liste des protocoles disponibles ?**  
    → FTP est un **protocole de couche application** qui utilise **TCP** au niveau transport.
    
3. **Quelle est la wildcard mask pour le sous-réseau 172.22.34.64/27 ?**  
    → **0.0.0.31** (l’opposé binaire du masque **255.255.255.224**).
    
4. **Pourquoi la commande `eq` est utilisée après la définition de l’adresse du serveur ?**  
    → Elle permet de **filtrer le trafic en fonction du port**, ici **eq ftp (port 21)**.
    
5. **Pourquoi n’est-il pas nécessaire de préciser un type d’ICMP ?**  
    → L’ACL **autorise tout le trafic ICMP**, ce qui inclut les pings et autres types de messages ICMP.
    
6. **Pourquoi la règle `deny any any` n’apparaît-elle pas dans l’ACL ?**  
    → Parce qu’un **deny implicite** est présent par défaut, tout ce qui n’est pas explicitement autorisé est rejeté.
    

---

#### **Étape 2 : Appliquer l’ACL sur l’interface appropriée**

1. **Sur quelle interface faut-il appliquer l’ACL 100 ?**  
    → **GigabitEthernet 0/0**, car c’est l’interface qui reçoit le trafic entrant depuis **PC1**.
    
8. **Dans quelle direction doit-on appliquer l’ACL ?**  
    → **"in" (entrée)**, car elle doit filtrer le trafic entrant depuis **PC1** vers le serveur.
    

---

#### **Étape 3 : Vérifier l’implémentation de l’ACL**

1. **Que doit-on vérifier si le ping entre PC1 et le serveur échoue ?**  
    → **Vérifier les adresses IP configurées** sur les appareils et l’interface concernée.
    
10. **Quelles sont les identifiants pour la connexion FTP au serveur ?**  
    → **Utilisateur : cisco | Mot de passe : cisco**.
    
11. **Pourquoi le ping entre PC1 et PC2 échoue ?**  
    → L’ACL **n’autorise pas explicitement la communication entre PC1 et PC2**, donc elle est bloquée par le **deny implicite**.
    

---

### **Partie 2 : Configurer, Appliquer et Vérifier une ACL Nommée Étendue**

#### **Étape 1 : Configurer une ACL pour autoriser HTTP et ICMP depuis PC2**

1. **Pourquoi faut-il utiliser une ACL étendue au lieu d’une ACL standard ?**  
    → Une ACL **étendue permet de filtrer en fonction des adresses source et destination, ainsi que des ports spécifiques**.
    
13. **Quel est le nom de l’ACL nommée ?**  
    → **HTTP_ONLY** (respecter la casse).
    
14. **Quelle est la wildcard mask pour le sous-réseau 172.22.34.96/28 ?**  
    → **0.0.0.15** (l’opposé binaire du masque **255.255.255.240**).
    
15. **Quel port est utilisé pour le trafic web (HTTP) ?**  
    → **80** (eq www).
    

---

#### **Étape 2 : Appliquer l’ACL sur l’interface appropriée**

1. **Sur quelle interface faut-il appliquer l’ACL nommée HTTP_ONLY ?**  
    → **GigabitEthernet 0/1**, car c’est l’interface qui reçoit le trafic de **PC2**.
    
17. **Dans quelle direction doit-on appliquer cette ACL ?**  
    → **"in" (entrée)**, car elle filtre le trafic venant de **PC2** vers le serveur.
    

---

#### **Étape 3 : Vérifier l’implémentation de l’ACL**

1. **Comment vérifier si l’ACL HTTP_ONLY est correctement configurée ?**  
    → Utiliser la commande **`show access-lists`**.
    
19. **Quels tests doivent réussir ?**
    

- **Le ping entre PC2 et le serveur** → **Réussi**.
- **L’accès HTTP (navigateur) de PC2 vers le serveur** → **Réussi**.
- **L’accès FTP depuis PC2 vers le serveur** → **Échec**, car **non autorisé par l’ACL**.

![[Pasted image 20250305135135.png]]


## Exercice : 5.4.13 Packet Tracer - Configure Extended IPv4 ACLs - Scenario 2

