# BRIEF VPN

##### Contexte du projet

WireGuard est un protocole de communication et un logiciel libre et open source permettant de créer un réseau privé virtuel. 

Dans votre SOC Lab avec votre Lead Tech Cybersecurity Officer :

Vous devez accéder à votre Cyberlab via un VPN par exemple WIREGUARD.

<span style="text-decoration:underline;">Première étape : POC</span>

Vous allez installer et tester WIREGUARD sur une VM PROXMOX "bac à sable"

Vous aurez besoin avec votre Team de pouvoir accéder uniquement depuis votre propre KL (via le WIREGUARD VPN) à votre VM PROXMOX "bac à sable"

<span style="text-decoration:underline;">Deuxième étape : PROD</span>

Vous allez installer WIREGUARD sur votre CyberLab Prod.

Votre équipe doit pouvoir depuis son propre KL accéder à distance à votre CyberLab Prod.

<span style="text-decoration:underline;">Schéma</span>

**![](https://lh3.googleusercontent.com/_6qsJwLhNpLvw4CqIUnvl35t-3A9UpKXTlJvA6fbBJrRKetl5mk-AR7BXCiSfiXn_NLL8pgp80kTjRcoToNT5DV3vfnbLdh0FKRZWFTWrPFwUvXH-BQkoWNKjm5gwGjrCVhwytux=s0)**


**_Si vous ne réussissez pas à installer Wireguard, nous vous recommandons de suivre les étapes suivantes:_**

#### Editer le fichier de sources.list 

Il s'agit du référentiel recommandé pour les tests et l'utilisation hors production. Ses packages ne sont pas aussi fortement testés et validés. Vous n'avez pas besoin d'une clé d'abonnement pour accéder au référentiel pve-no-subscription .

Nous vous recommandons de configurer ce référentiel dans /etc/apt/sources.list :

 `/etc/apt/sources.list.d/ceph.list`

<span style="text-decoration:underline;">Dans le fichier (via nano par exemple) :</span>


> deb http://ftp.debian.org/debian bullseye main contrib
>deb http://ftp.debian.org/debian bullseye-updates main contrib
> \# PVE pve-no-subscription repository provided by proxmox.com,
>\# NOT recommended for production use
>deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
>\# security updates
> deb http://security.debian.org/debian-security bullseye-security main contrib
	
Ensuite on modifie le fichier _/etc/apt/sources.list.d/pve-enterprise.list_

On souhaite modifier l’adresse qui n’est pas accessible à cause d’un dépôt entreprise pour celà on modifie comme Sonam Jamtsho<sup><a href="https://sjamso.blogspot.com/2021/01/solvedproxmox-update-e-e-failed-to.html">1</a></sup>:

<span style="text-decoration:underline;">To do that replace the repository:</span>


   >deb http://enterprise.proxmox.com/debian/pve buster pve-enterprise


  with following:


>    deb http://download.proxmox.com/debian/pve buster pve-no-subscription

<span style="text-decoration:underline;">Mises à jour :</span>

`apt update` 

`apt upgrade`

#### Installation Wireguard : Serveur & Client

Nous avons un serveur bac à sable Proxmox et un client Kali Linux.

`apt install wireguard`

<span style="text-decoration:underline;">Vérifier le type de version :</span>

`wg --version`

Nous avons la version v1.0.20210223

#### Création de clés privée et publique : Serveur & Client

Wireguard utilise des outils genkey pour générer des clés privée et publique : et on va l'éditer dans un fichier qu'on va le nommer “publickey”. Dans le répertoire .ssh/ nous avons saisi cette commande :

`wg genkey | tee privatekey | wg pubkey > publickey`

BE CAREFUL: La **privatekey** ne doit jamais être partagée car elle permet de déchiffrer tous les paquets réseaux envoyés par le client à la réception et inversement, elle permet de chiffrer à l'envoi.

Possibilité de les supprimer, une fois que les fichiers de configuration sont modifiés Client & Serveur.

#### Configuration de l'interface Réseau côté Serveur :

Maintenant, nous allons créer un **tunnel** d’interface pour la configuration de wireguard. 

Pour cela, nous allons éditer le fichier wg0.conf

`nano /etc/wireguard/wg0.conf` 

<span style="text-decoration:underline;">En sortie :</span>

>[Interface]
>PrivateKey=&lt;clef privée server>
>Address=10.0.0.1/24  
 >SaveConfig=true
>PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o
  >vmbr0 -j MASQUERAVDE;
  >PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o vmbr0 -j MASQUERADE;
   >ListenPort = 51820

<span style="text-decoration:underline;">Remarque : </span>

Adress=10.0.0.1/24  : On peut utiliser n'importe quelle adresse IP Privée, juste il faut bien être sûr qu'elle soit unique, car si on utilise la même adresse IP qu’on utilise ailleurs ça engendre des problèmes de routage.

<span style="text-decoration:underline;">Démarrer l'interface réseau :</span>

Dans le répertoire : /etc/wireguard

`wg-quick up wg0` 

<span style="text-decoration:underline;">Vérification:</span>

`sudo wg`

<span style="text-decoration:underline;">Vérifier la création du tunnel réseau avec la commande :</span>

`ip link`
<pre>

root@sandbox0CYBERLAB6:~# ip link
1: lo: &lt;LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
	link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master vmbr0 state UP mode DEFAULT group default qlen 1000
	link/ether 00:15:5d:03:99:18 brd ff:ff:ff:ff:ff:ff
3: vmbr0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
	link/ether 00:15:5d:03:99:18 brd ff:ff:ff:ff:ff:ff
<b>5: wg0: &lt;POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000

link/none </b>
</pre>
#### Configuration de l'interface Réseau côté Client :

`sudo nano /etc/wireguard/wg0.conf`

>[Interface]
>PrivateKey=&lt;clef privée client> 
 >Address=10.0.0.1/32
 SaveConfig=true
>
  >  [Peer]
PublicKey=&lt;clef publique server> 
Endpoint=&lt;Adresse IP publique du serveur>:51820
AllowedIPs=0.0.0.0/0 

<span style="text-decoration:underline;">Remarques:</span>

**0.0.0.0/0 **: Le serveur acheminera tout le trafic du client à travers ce tunnel même si la destination est un trafic Internet normal.

**PEER** : Demander au client de se connecter au serveur 

**51820** : TCP/UDP couche transport, c’est le port dédié pour Wireguard, il sert à transférer rapidement et vérifier les fichiers.

<span style="text-decoration:underline;">Démarrer l'interface réseau :</span>

Dans le répertoire : /etc/wireguard

`wg-quick up wg0` 

<span style="text-decoration:underline;">Vérification:</span>

`sudo wg`
On vérifie l'existence de la carte réseau wg.

#### Ajouter les Clients au Serveur :

Ensuite, vous devez ajouter les clients au fichier de configuration du serveur sinon, le tunnel ne sera pas établi. Remplacez &lt;client-public-key> par la clé publique générée par le client et &lt;client-ip-address> par l'adresse IP du client sur l'interface wg0.

`wg-quick down wg0`

`nano /etc/wireguard/wg0.conf`

<span style="text-decoration:underline;">Sortie:</span>


   >[Interface]
PrivateKey=&lt;clef privée serveur> 
Adress=10.0.0.1/24
SaveConfig=true
>
>[Peer] #Client1
PublicKey=&lt;clef publique client> 
Endpoint=&lt;Adresse IP publique du client>:51820
AllowedIPs=10.0.0.2/32
>
>[Peer] #Client2
PublicKey=&lt;clef publique client> 
Endpoint=&lt;Adresse IP publique du client>:51820
AllowedIPs=10.0.0.3/32
>
>[Peer] #Client3
PublicKey=&lt;clef publique client> 
Endpoint=&lt;Adresse IP publique du client>:51820
AllowedIPs=10.0.0.4/32

`wg-quick up wg0`

#### La commande ip_forward pour le Serveur :

On saisit la commande suivante:

`cat /proc/sys/net/ipv4/ip_forward`

Si le résultat est 0, saisir la commande suivante :

`sysctl -w net.ipv4.ip_foward=1`

#### Test :

_Côté Client :_

`sudo ping &lt;Adresse IP Serveur> `

_Côté Serveur :_

`tcpdump -envi wg0 host &lt;Adresse IP Serveur>`

Bonus 

Start wg iface on boot : 
`systemctl enable wg-quick@wg0`

## PROD

<span style="text-decoration:underline;">Schéma:</span>

**![](https://lh3.googleusercontent.com/0u1hRTygfQrWb8XxHoBepkRCW1Mvd2Jqilwa15Gy9Hr6MoJpgInA_RGabxfzQLh1AiGb8ap4lRMrrUGC4p5lskXkB8hjQR_9kzEZUZZh1dKNSALUMwJ-tjQwkKGH2ag4J0cYyFw7=s0)**

Premièrement on se connecte au CyberLAB6 en ssh depuis un poste administrateur ou par putty depuis la machine hôte windows:

Notre IP publique est 5.39.65.74/24



* Depuis kali :

ssh toto@5.39.65.74



* Connexion par ssh avec Putty sur Windows:

**![](https://lh4.googleusercontent.com/Eyr0iQAbJtiXokUKX0P8HxmYWwEShlwTQNQ_ezxxdE5htspR0EdcXtjnvwrNy_t2ZowiLifWOO8HvakMEI3f6NOQGbsZBmD0GuZGCTuXeiRgC9ZIWHZPdwbXBhJMWYf8QLFrRyJl=s0)**

Notre utilisateur Toto a les droits sudo sur le serveur. Avant de commencer,  on se connecte en root sur le serveur pour des soucis de permissions d’accès pour l’installation et la configuration du serveur Wireguard. 

 

Ajouter dans le fichier “souces.list” le lien suivant :

>deb http://deb.debian.org/debian buster-backports main

sauvegarder, quitter, puis installer wireguard :

`apt update && apt full-upgrade`
`apt install wireguard`
`apt install pve-headers-5.4.140-1-pve`
`apt update && apt upgrade`
`dkms autoinstall`
`modprobe wireguard`

Créer le jeu de clefs du serveur dans /etc/wireguard :

`wg genkey | tee privatekey | wg pubkey > publickey`

**![](https://lh4.googleusercontent.com/DIzAHIArEJuwEClrIjX6qkHUoGvTNMFcPsc5VXyInTrWpkiJ0UJvFOUpPhpRtqIfNSHWAIXpMRUQA8h40vuDjI-QGdtINr8QbSnh922rol7p_9F7VlJUQr2key61HpvXJxp51sXY=s0)**

Pour des raisons de sécurité on coupe l’accès à la lecture (**r**) , écriture (**w**), et l'exécution (**x**). On souhaite restreindre (surtout pour la clé privée) la lecture et la modification de nos clées à l’unique propriétaire , ici le root. L'exécution d’une clef ne donnerais pas grand chose donc nous la désactivon également pour le root:

`chmod 600 publickey privatekey`

**![](https://lh3.googleusercontent.com/1HscoVCRlRqBoNrEIR5lQ1_UZUNYt_lPjQKJ0YcoGrIAZabP0kSGfilXsCkMx9EUDKAZ8NSZPtVKJutXCTn6wIUpmW8weaRfujfwuoPONMh0XQ7vweglpJuO8eoWCKnIM0d_yk3t=s0)**

Désormais seul le Root peut consulter et écraser nos belles clefs.

Depuis que l’on as protégé le répertoire /etc/wirguard la configuration demandera un accès root connecter vous en root à votre serveur client:



* su -i
* sudo -i

_au choix._

**<span style="text-decoration:underline;">Configuration CLIENT</span>**

Nous avons créé un nouveau fichier wg1 dans **/etc/wireguard/** pour la configuration client afin de ne pas confondre avec le fichier wg0 qui concerne la configuration client pour notre serveur bac à sable:

`nano /etc/wireguard/wg1.conf` 


   >[Interface]
PrivateKey=&lt;clef privée client> 
Adress=10.10.0.1/32
SaveConfig=true
>
>[Peer]
PublicKey=&lt;clef publique server> 
Endpoint=&lt;Adresse IP publique du serveur>:51820
AllowedIPs=0.0.0.0/0 

**<span style="text-decoration:underline;">Configuration SERVEUR</span>**

`nano /etc/wireguard/wg0.conf`

<span style="text-decoration:underline;">Sortie:</span>


   >[Interface]
PrivateKey=&lt;clef privée serveur> 
Address=10.10.0.1/24
SaveConfig=true
>
>[Peer] #Client1
PublicKey=&lt;clef publique client> 
AllowedIPs=10.10.0.3/32
>
>[Peer] #Client2
PublicKey=&lt;clef publique client> 
AllowedIPs=10.10.0.5/32
>
>[Peer] #Client3
PublicKey=&lt;clef publique client> 
AllowedIPs=10.10.0.6/32

`wg-quick up wg0`

## Phases de tests

**RESULTAT CLIENT 3 PING LE SERVEUR VPN**

client: 
`ping 10.10.0.1`

serveur:
`tcmpdump -envi wg0 host 10.1O.0.1`

**![](https://lh3.googleusercontent.com/03uLz7WlsL9P1OIOdw4DoQxj8iPVgfZDI_D3lbHzJvRHABz6IBK-Bktm212CSCbSQ0WG3zMSM62183R9g2fp4SAVHnOGRoDW6ZV0m0EJl7NUGAADAXU-BC3HOQW4DtmzQHNwF-B7=s0)**


**RÉSULTAT SERVEUR PING GOOGLE ** 

 **![](https://lh3.googleusercontent.com/BKLvK3vV90HqxVQLQqLegw8JuhA4unx_Jsjotz94QEO1dvI0StXTeijtKAq7xx5ZJaBrIadrnyBrwjGXloEgS-8Abpxtr35q_NkK7s1HqHNJPXHvnGL-x_LV0xi3J5J1RzsGU-1O=s0)**


**CLIENT PING GOOGLE** 

**![](https://lh5.googleusercontent.com/Xa-0BSmzUcZ7HJUTDx4KXRLTksYQ9jzWUJtnxbLfZuOvoZcO3oL0qsO0XXWqNUocRaZf6066GVPgz5CY4mmThqVswmZldGmaxee6YiLTwS3rrwRajViy3--XarRlpn7tE7-xKoIG=s0)**

**CONNEXION SSH CLIENT 2 VERS CLIENT 3**
`sudo ssh username@public.ip`

**![](https://lh3.googleusercontent.com/zzjYPEOvFPCbDiYUQLwWFfHOhfc5vuiPaclCf9DTgo2zS5HLRLEJ4H9AtLGobQ0ehvoeNLlu9mQW1cg6Z923zfKjxlMBjy8CMFDKHnopNBGJtegk6p_gQ6wsSVrcAm16ctx7PwWN=s0)**


Ressources:



* configuration de source.list

    [https://securityboulevard.com/2019/04/howto-install-wireguard-in-an-unprivileged-container-proxmox/](https://securityboulevard.com/2019/04/howto-install-wireguard-in-an-unprivileged-container-proxmox/) 


    [https://backports.debian.org/Instructions/](https://backports.debian.org/Instructions/) 

* [guide complet débutant sur un client ubuntu](https://www.the-digital-life.com/wireguard-installation-and-configuration/) par: [The Digital Life](https://www.youtube.com/c/TheDigitalLifeTech/videos)
* [guide complet d’installation avec les actions détaillé](https://www.smarthomebeginner.com/linux-wireguard-vpn-server-setup/)