
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





Exercice : 5.1.8 Packet Tracer - Configure Numbered Standard IPv4 ACLs



Objectives
Part 1: Plan an ACL Implementation

Part 2: Configure, Apply, and Verify a Standard ACL

Background / Scenario
Standard access control lists (ACLs) are router configuration scripts that control whether a router permits or denies packets based on the source address. This activity focuses on defining filtering criteria, configuring standard ACLs, applying ACLs to router interfaces, and verifying and testing the ACL implementation. The routers are already configured, including IP addresses and Enhanced Interior Gateway Routing Protocol (EIGRP) routing.

Instructions
Part 1: Plan an ACL Implementation
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

# Tout les ping marche bien


c.     Issue the show access-lists command again on routers R2 and R3. You should see output that indicates the number of packets that have matched each line of the access list. Note: The number of matches shown for your routers may be different, due to the number of pings that are sent and received.

R2# show access-lists

Standard IP access list 1

10 deny 192.168.11.0 0.0.0.255 (4 match(es))

20 permit any (8 match(es))

 

R3# show access-lists

Standard IP access list 1

10 deny 192.168.10.0 0.0.0.255 (4 match(es))

20 permit any (8 match(es))




Exercice : 5.1.9 Packet Tracer - Configure Named Standard IPv4 ACLs


Objectives
Part 1: Configure and Apply a Named Standard ACL

Part 2: Verify the ACL Implementation

Background / Scenario
The senior network administrator has asked you to create a standard named ACL to prevent access to a file server. The file server contains the data base for the web applications. Only the Web Manager workstation PC1 and the Web Server need to access the File Server. All other traffic to the File Server should be denied.

Instructions
Part 1: Configure and Apply a Named Standard ACL
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

Part 2: Verify the ACL Implementation
Step 1: Verify the ACL configuration and application to the interface.
Open configuration window

Use the show access-lists command to verify the ACL configuration. Use the show run or show ip interface fastethernet 0/1 command to verify that the ACL is applied correctly to the interface.

Step 2: Verify that the ACL is working properly.
All three workstations should be able to ping the Web Server, but only PC1 and the Web Server should be able to ping the File Server. Repeat the show access-lists command to see the number of packets that matched each statement.



