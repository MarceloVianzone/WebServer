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
5. Dividiamo i vari siti
    - cd /etc/apache2/sites-avaiable
    - copia il file *000-default.conf* e dagli un altro nome, nel mio caso *100-default.conf*
    - ora modifichiamo il secondo file (*100-default.conf*). Aggiungiamo una stringa sopra ServerAdmin
    - ServerName il_tuo_dns_primario<br/>
    ```yaml
        ServerName marsy01.ddns.net
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
    ```
    - ed ora cambiamo la directory di questo sito (*DocumentRoot*)
    - io ho deciso di dargli una directory con il nome del dns
    ```yaml
        ServerName marsy01.ddns.net
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/marsy01
    ```
    - il risultato è il seguente
    ```yaml
    <VirtualHost *:80>
            # The ServerName directive sets the request scheme, hostname and port that
            # the server uses to identify itself. This is used when creating
            # redirection URLs. In the context of virtual hosts, the ServerName
            # specifies what hostname must appear in the request's Host: header to
            # match this virtual host. For the default virtual host (this file) this
            # value is not decisive as it is used as a last resort host regardless.
            # However, you must set it for any further virtual host explicitly.
            #ServerName www.example.com
            
            ServerName marsy01.ddns.net
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/marsy01
            
            # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
            # error, crit, alert, emerg.
            # It is also possible to configure the loglevel for particular
            # modules, e.g.
            #LogLevel info ssl:warn
            
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
            
            # For most configuration files from conf-available/, which are
            # enabled or disabled at a global level, it is possible to
            # include a line for only one particular virtual host. For example the
            # following line enables the CGI configuration for this host only
            # after it has been globally disabled with "a2disconf".
            #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
6. Esegui il passo 5 con quanti dns vuoi
    - ricordati di cambiare il dns
    - ricordati di cambiare la directory
7. Abilitiamo i siti
    - 
    
