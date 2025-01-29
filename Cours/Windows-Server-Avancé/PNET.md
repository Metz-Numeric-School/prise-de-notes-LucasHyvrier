


Créer une machine virtuel 

# Commande pour créer un disque virtuel


cd /opt/unetlab/addons/qemu/opnsense-24.7/  
  
/opt/qemu/bin/qemu-img create -f qcow2 virtioa.qcow2 10G  
  
cd  
  
  
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions