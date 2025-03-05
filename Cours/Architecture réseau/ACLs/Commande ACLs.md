

## **Étape 1 : Configurer une ACL standard numérotée**

Les ACLs standards filtrent le trafic en fonction de l’adresse IP source uniquement. Cisco recommande de les placer aussi près que possible de la destination.

### **1. Création de l’ACL sur R3**

Cette ACL permet aux réseaux **192.168.10.0/24** et **192.168.20.0/24** d’accéder au réseau **192.168.30.0/24**.  
Toute autre adresse IP est refusée par défaut avec **deny any**.

```bash
R3(config)# access-list 1 remark Autoriser l’accès des LANs de R1
R3(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R3(config)# access-list 1 permit 192.168.20.0 0.0.0.255
R3(config)# access-list 1 deny any
```

📌 **Explication :**

- **access-list 1 remark** → Ajoute un commentaire pour documenter la liste d’accès.
- **permit 192.168.10.0 0.0.0.255** → Autorise tout le sous-réseau 192.168.10.0/24.
- **permit 192.168.20.0 0.0.0.255** → Autorise tout le sous-réseau 192.168.20.0/24.
- **deny any** → Bloque tout autre trafic non mentionné explicitement.

---

### **2. Appliquer l’ACL à l’interface appropriée**

On applique cette ACL en sortie (**out**) sur **l’interface GigabitEthernet 0/0/0** de R3.

```bash
R3(config)# interface g0/0/0
R3(config-if)# ip access-group 1 out
```

📌 **Explication :**

- **ip access-group 1 out** → Applique la liste d’accès numéro **1** en sortie (**out**) de l’interface.

---

### **3. Vérifier l’ACL**

Pour voir la liste des règles définies dans l’ACL **1** :

```bash
R3# show access-lists 1
```

Pour vérifier sur quelle interface l’ACL est appliquée et dans quelle direction :

```bash
R3# show ip interface g0/0/0
```

📌 **Explication :**

- **show access-lists 1** → Affiche les règles de l’ACL numéro 1.
- **show ip interface g0/0/0** → Montre les paramètres de l’interface, y compris les ACLs appliquées.

---

### **4. Tester l’ACL**

Vérifier si les hôtes autorisés peuvent accéder au réseau **192.168.30.0/24**.

1. **Depuis PC-A (192.168.10.x) → ping PC-C (192.168.30.x)**
    
    ```bash
    PC-A> ping 192.168.30.3
    ```
    
2. **Depuis PC-B (192.168.20.x) → ping PC-C (192.168.30.x)**
    
    ```bash
    PC-B> ping 192.168.30.3
    ```
    
3. **Depuis PC-D (réseau non autorisé) → ping PC-C**
    
    ```bash
    PC-D> ping 192.168.30.3
    ```
    
4. **Depuis R1 → ping PC-C**
    
    ```bash
    R1# ping 192.168.30.3
    ```
    

📌 **Explication :**

- Si les pings des réseaux **192.168.10.0/24** et **192.168.20.0/24** réussissent, l’ACL fonctionne comme prévu.
- Si les pings de **PC-D** échouent, c’est normal car son réseau n’est pas autorisé.
- Le ping depuis **R1** doit échouer si son IP ne correspond pas aux réseaux autorisés.

---

## **Étape 2 : Configurer une ACL standard nommée**

Cette ACL permet :

- Au réseau **192.168.40.0/24** d’accéder au **192.168.10.0/24**.
- Seulement **PC-C (192.168.30.3)** peut aussi accéder au **192.168.10.0/24**.

### **1. Création de l’ACL nommée sur R1**

On utilise le nom **BRANCH-OFFICE-POLICY**.

```bash
R1(config)# ip access-list standard BRANCH-OFFICE-POLICY
R1(config-std-nacl)# permit host 192.168.30.3
R1(config-std-nacl)# permit 192.168.40.0 0.0.0.255
R1(config-std-nacl)# end
```

📌 **Explication :**

- **ip access-list standard BRANCH-OFFICE-POLICY** → Crée une ACL nommée.
- **permit host 192.168.30.3** → Autorise **uniquement PC-C** à accéder à **192.168.10.0/24**.
- **permit 192.168.40.0 0.0.0.255** → Autorise tout le sous-réseau **192.168.40.0/24**.

💡 **Autre façon d’écrire la première règle :**

```bash
R1(config-std-nacl)# permit 192.168.30.3 0.0.0.0
```

C’est équivalent à **permit host 192.168.30.3**.

---

### **2. Appliquer l’ACL sur l’interface GigabitEthernet 0/0/0 de R1**

```bash
R1(config)# interface g0/0/0
R1(config-if)# ip access-group BRANCH-OFFICE-POLICY out
```

📌 **Explication :**

- **ip access-group BRANCH-OFFICE-POLICY out** → Applique l’ACL **en sortie** sur l’interface.

---

### **3. Vérifier l’ACL**

```bash
R1# show access-lists
```

```bash
R1# show ip interface g0/0/0
```

📌 **Explication :**

- **show access-lists** → Affiche les ACLs configurées sur le routeur.
- **show ip interface g0/0/0** → Vérifie l’application de l’ACL.

---

### **4. Tester l’ACL**

1. **Depuis PC-C (192.168.30.3) → ping PC-A (192.168.10.x)**
    
    ```bash
    PC-C> ping 192.168.10.3
    ```
    
2. **Depuis R3, tester en faisant un ping étendu avec source 192.168.30.1 vers PC-A**
    
    ```bash
    R3# ping
    Protocol [ip]: 
    Target IP address: 192.168.10.3
    Extended commands [n]: y
    Source address or interface: 192.168.30.1
    ```
    
3. **Depuis PC-D (192.168.40.x) → ping PC-A**
    
    ```bash
    PC-D> ping 192.168.10.3
    ```
    



