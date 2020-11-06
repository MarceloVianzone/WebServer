# Ubuntu Server Apache2
1. Installare openssh
    - (utente root)     apt install openssh-server
    - (Utente non root) sudo apt-get install openssh-server
2. Installare Apache2
    - (utente root)     apt-get install apache2
    - (utente non root) sudo apt.get install apache2
3. Impostare ip statico
    - (utente root)     nano /etc/netplan/ *nome file*
    - (utente non root) sudo nano /etc/netplan/ *nome file*<br/>
        Ci sono svariati nomi di questi file nel mio caso è *01-netcfg.__yaml__*.
        Comunque vengono tutti distinti dall'estensione **yaml**.
        Una volta entrati dentro l'editor modifichiamo questo file in questo modo:
        ```
        network:
          version: 2
          renderer: networkd
          ethernets:
            eth0:
              dhcp4: false
              addresses: [inserisci_qui_il_tuo_ip / metti la netmask espressa in numero (es 24)]
              gateway4: inserisci_qui_il_tuo_gateway_predefinito
              nameservers:
                  addresses: [inserisci_i_tuoi_dns(es 8.8.8.8) , (es 1.1.1.1)]
        ```
4. Rendiamo effettiva la modifica 
    - (utente root)     netplan apply
    - (utente non root) sudo netplan apply
5.
