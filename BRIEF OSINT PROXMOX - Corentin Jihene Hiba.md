# BRIEF OSINT PROXMOX

## Prérequis:

### ELEMENT :



* [https://doc.ubuntu-fr.org/element](https://doc.ubuntu-fr.org/element)
* Element nous permet de récupérer le fichier KDBX contenant les login/motsdepasse.

### Keepass :



* apt install keepassxc
* Keepassxc start
* Keepass permet de retrouver les accès de serveur de manière sécurisée.

## Projet:

En alternance Cyber dans la cellule CERT, vous êtes en charge de :


#### Mettre en place un Keepass de manière à sécuriser et retrouver les accès de votre serveur:

   `sudo apt-get install keepassxc`


#### Permettre l'accès sécurisé en SSH depuis le terminal de votre KL au SRV dédié OVH CYBERLAB:

  `ssh -v`

  `ssh root@ns3186435.ip-5-39-65.eu` 

    [sudo] password for User Kali : le récupérer depuis Keepass

    root@ns3186435.ip-5-39-65.eu's password server : 

#### Accéder sur un Browser à Proxmox VE 6 installé sur ce SRV dédié:

* Renseigner l’adresse du serveur dans un navigateur web et ajouter le numéro de port: 8006 (par défaut) pour avoir accès à Proxmox.

Pour se connecter au serveur :

* Récupérer depuis keepass

Adresse :

[https://ns3186435.ip-5-39-65.eu:8006](https://ns3186435.ip-5-39-65.eu:8006)

Mot de passe :****

#### Enumérer depuis votre Terminal KL toutes les infos/renseignements que vous pourrez obtenir sur l'Interface WEB PROXMOX de votre SRV dédié OVH:

`ls -a`

_Les dossier sont cachées_

Seul les répertoires .gnupg/ et .ssh/ sont accessibles

`lspci` : Informations système du serveur.

`lscpu` : Avoir des informations sur le processeur. 

`cat /proc/cpuinfo` : Obtenir des informations supplémentaires sur le processeur. 

`smartctl -i /dev/sdb `:  Nous informe sur les disques durs et SSD.

`ip a : `Cette commande permet d’avoir des informations sur l’adresse ip, adresse broadcast...


`uname -a `: Affiche au complet les informations système.

* Les options suivantes permettent d’affiner:

`-s `Kernel Name

`-n `Network Node Name (Hostname)

`-r `Kernel Release

`-v `Kernel Version

`-i `Hardware Platform (OS architecture)

`-o `Operating System

`ls `: Représente la liste des fichiers d'un dossier

Options :
` -a` Pour les fichiers cachés
`-l `Pour la liste détaillée
`-h `Pour les tailles en unités "human readable", l'option 
`-R` Permet de visualiser les sous-dossiers.


`ls -all `: Affiche tous les fichiers (cachés compris) du répertoire courant

`ls -l ` : En  plus  du  nom, afficher le type du fichier, les permissions d'accès, le nombre de liens  physiques,  le  nom  du propriétaire et du groupe, la taille en octets, et l'horodatage.

`ls -s `: Affiche la taille de chaque fichiers

` ls -la `: Permet d'afficher tout ce qui se trouve à l'intérieur du dossier car par défaut les dossiers et les fichiers qui sont cachés ne sont pas affichés.  
->  On remarque que tous les fichiers cachés sont précédés d’un . et ça veut dire que par défaut il va être caché par le système d’exploitation.

On a la lettre d---- pour signifier qu'il s'agit d'un dossier qui va contenir d'autres choses, et si on n’a rien ---- ça veut dire qu'il s’agit d'un fichier. 

`du` : Disk usage, précise l'espace disque que prend chaque fichier ou dossier (l'option `-h` permet d'obtenir les tailles en "human readable")

`free -m` : Le résumé de l'occupation mémoire et fichier d'échange.

`ps aux` : Affiche la Liste les processus en cours.

`netstat` : Affiche des informations et statistiques sur les connexions réseau, la table de routage, etc. 

 `top` : Affiche les processus en temps réel.



`w` : Affiche les connections active au serveur.

La commande : `dmidecode` : Pour en savoir un peu plus sur son matériel.

`dmidecode -q -t memory | grep Size`: Affiche un module de 16 Go de mémoire vive, et la machine peut en accueillir 4 autres.

Une autre option utile est `-s`, par exemple si on recherche des informations sur son processeur :

Pour chercher des informations sur le système et le baseboard , on a l'option `-s`

`sudo dmidecode –type 17` : Affiche les détails sur les barrettes de mémoires.

* Création d’un utilisateur sudo:

`adduser username`

`usermod -aG sudo username`

* Pour supprimer un utilisateur: 

`userdel username`

* La commande uptime:
 `uptime `: Permet d’afficher le résultat suivant :

Les options disponibles pour cette commande sont les suivantes :

`-p` (--pretty) pour afficher au format plus compréhensible

`-h` (--help) pour afficher l’aide en ligne

`-s` (--since) pour afficher le temps au format {yyyy-mm-dd HH:SS}

`-V` (--version) pour afficher la version du logiciel

<br/><br/>

#### RESSOURCES
DOC TECHNIQUE (avec des captures d'écran) : https://docs.google.com/document/d/136n6fY-tpw-7MiaFwVyHcGTHvqsRCwQTiTnrhNgP8Vs/edit?usp=sharing
