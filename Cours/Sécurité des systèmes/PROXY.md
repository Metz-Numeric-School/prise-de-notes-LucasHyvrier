
Créer une machine virtuel dans PNET

# Commande pour créer un disque virtuel

Connectez vous sur putty et entré les commandes si dessous :

cd /opt/unetlab/addons/qemu/opnsense-24.7/  
  
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G  
  
cd  
  
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions


Grace a cela on pourras créer des VM dans pnet directement.


2
1
N
192.168.1.254 
/24

n
n
y
192.168.1.50
192.168.1.100
N
N
N
