## TP5 Service réseau

### Exercice 2 : Préparation de l'environnement

Commandes : 

* *ip a* => affiche l'ensemble des modules et addresses ip associés présents sur la machine
* *ifconfig* => équivalent de *ip a* (nécessite paquet spécifique **net-tools**) 

Le résultat affichée par cette même commande est :

1. *lo* => loopback => interface virtuel connecté au système afin de pouvoir réaliser un ensemble de test de connectivité utile

2. *enp0s3* => interface physique (carte ethernet) présente sur la machine

### Exercice 3 : Installation du serveur DHCP

Commandes :

* *sudo apt install isc-dhcp-server*  => installer le paquet pour le serveur DHCP
* *systemctl status isc-dhcp-server*  => observer le statut du serveur DHCP
* *sudo ip addr add @IP dev moninterface*  => ajoute une adresse ip à l'interface désigné
* *sudo ip addr flush moninterface* => efface l'adresse ip d'une interface

La configuration d'un serveur DHCP se fait au travers du fichier **/etc/dhcp/dhcpd.conf**.

Exemple de configuration :

    default-lease-time 120; #lease-time = temps qu'une machine en réseau 
                            #peut utiliser une IP du réseau
    max-lease-time 600;     #max= valeur max de temps que la machine gardera l'adresse IP
                            #ici 10 min max (600 secondes = 10 mins)
    authoritative;#DHCP officiel pour notre réseau
    option broadcast-address 192.168.100.255;          #informe les clients de l'adresse de 
                                                       #broadcast
    option domain-name "tpadmin.local";                #tous les hôtes qui se connectent au
                                                       #réseau auront ce nom de domaine
    subnet 192.168.100.0 netmask 255.255.255.0 {      #configuration du sous-réseau 192.168.100.0
    range 192.168.100.100 192.168.100.240;            #pool d'adresses IP attribuables
    option routers 192.168.100.1;                     #le serveur sert de passerelle par défaut
    option domain-name-servers 192.168.100.1;         #le serveur sert aussi de serveur DNS
    }

**NB** : Les valeurs indiquées sur les deux premières lignes sont faibles, afin que l’on puisse voir constituer quelqueslogs durant ce TP. Dans un environnement de production, elles sont beaucoup plus élevées!

Il faut par la suite éditer le fichier/etc/default/isc-dhcp-serverafin de spécifier l’interface sur laquelle le serveur doit écouter.

Pour valider la configuration, il faut utiliser la commande *dhcpd -t*. Par la suite, il faut redémarrer le serveur avec la commande *systemctl restart isc-dhcp-server*.








