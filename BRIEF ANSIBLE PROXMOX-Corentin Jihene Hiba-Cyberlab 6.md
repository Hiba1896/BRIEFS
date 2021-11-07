# BRIEF
# DÉPLOIEMENT AUTOMATISÉ ANSIBLE


## Contexte de Projet 

Configuration ANSIBLE



* Installation sur Kali

Connexion à distance



* Générer une clé ssh : privée pour Kali et publique pour proxmox

Création de Playbooks : fichier YAML dans lesquels sont mentionnés toutes les tâches qu'Ansible doit exécuter. De la configuration de l'environnement informatique de production jusqu'à l'orchestration de toutes les étapes de déploiement d'une application ou d'une mise à jour sur celui-ci.



* inventory
* main.yml


![|920x720](https://lh4.googleusercontent.com/LGiMPrtv1NnE2i0zrsmLjIJG-HH2D2LQwg2TAUibrbnVRw0nCpkO3-M4sqPJ18xup34_DMJMzzuUbRQ-2TEaBJMtnuSMUWcSRvX6_7HPp4kIkxkytnbTDBevvsEay1-20Hz0M9ES=s0)

# **Prérequis**

Installation de **Proxmox** sur une VM **NODE**

<pre>
* Proxmox version ve-7.0-2
* Type linux
* Version debian 64
* Mémoire 2 Go
* Disque 30 Go
* Réseau accès par pont --- mode promiscuité allow all
* Installer proxmox VE 
</pre>

Création d’un **NODE MANAGER **sur** Kali** (préalablement installée)



* Ouvrir le terminal de kali et faire update & upgrade (voir les commandes ci-dessous)
* Récupérer l’adresse IP depuis la VM Proxmox, par exemple: [https://192.168.X.X:8006](https://192.168.X.X:8006)
* Se connecter depuis un navigateur web sur Kali à l’interface Proxmox

# **Déploiement Ansible**

#### Les opérations de déploiement



* Un **rôle** contient un ou plusieurs fichiers de configuration (YAML) 
* Un fichier de configuration contient une ou plusieurs **tâches** 
* Une tâche fait appel à un **module**

Le YAML (Yet Another Markup Language). YAML permet d’écrire des structures de données qui peuvent être spécifiées sous la forme de listes.

#### Installation Ansible 

Ansible est une application que nous allons utiliser pour configurer deux sous-réseaux à distance: VMBR1 & VMBR2.



* Installer **Ansible** sur Kali

   `~$ sudo apt-get update`


   `~$ sudo apt-get upgrade`


   `~$ sudo apt-get install ansible`

* Vérification de l’installation :pour afficher le fichier exécutable

   `~$ “cd /bin && ls”`


_**À partir de cette étape, toutes les manipulations se feront sur Kali depuis le terminal.**_

#### **Environnement virtuel**

Nous avons testé la création d’un environnement virtuel :

Vérification de la version **Python**



* Vérifier quelle version de python existe dans /bin : 

   `~$ cd /bin && ls | grep python`


Installation** Python Virtual Environment**



* En tant que root : 

    `~# apt-get install python3-virtualenv` (veiller à bien mentionner le **numéro** de version).


Ajout d’un utilisateur “ansible”:



* Ajouter un utilisateur : 

    `~# adduser bibansible/corensible/jiheneciible` 

* Nous avons choisi un MP unique pour tout le monde

Création de l’environnement :



* Se connecter en tant qu’utilisateur ansible créé dans la partie précédente, exemple

    (switch user) :


   `~$ su jihencible`

* Le répertoire courant est celui de l’utilisateur précédent on fait donc: 

   `~$ cd`


**![](https://lh5.googleusercontent.com/SUC79DFAL3FSV0aOY1LSl09edrvvdkb9ftIe5mfEEm2Ee3YLn0djeHbz51yaNuEzgCFukcVK7gz3VctznsIUi81EIsOEuqh00fRxkiAmoRDfXjeFmqByU_ka1L8ZxznImHqXMST3=s0)**

#### Créer l’environnement virtuel 

   `~$ virtualenv ansible2.10.8`
	
_C’est juste le nom du répertoire créé et non pas la version exacte, par la suite on utilisera la version 2.7.10_

* Activer et vérifier l’environnement virtuel :

   `~$ source ansible2.10.8/bin/activate`


Ensuite, nous avons installé Ansible dans cet environnement :



* Installer Ansible dans l’environnement virtuel: 

   `~$ pip3 install ansible==2.7.10` 


Vérifier l’installation de **l’environnement virtuel**



* Commande pour la vérification: 

   `~$ ls ansible2.10.8/bin/ansible* -l`

* Spécifier le nom du répertoire qui a été créé et non pas la version de l’application qui a été installé

Vérifier la communication entre le Node et le Node Manager


   `~$ ssh root@addresse-IP-Proxmox`

Retour sur kali:


   `~$ exit`

#### Fork

Spécifier l'emplacement de notre travail pour pouvoir le cloner depuis Github

* Aller sur [GitHub](https://github.com/Hiba1896/ansible-proxmox/)
* Appuyer sur Fork 
* Copier le lien https de Github

  `~$ cd home/username/Downloads/`


  `~$ git clone [https://github.com/jihene-battikh/ansible-proxmox.git](https://github.com/jihene-battikh/ansible-proxmox.git)`


**![](https://lh6.googleusercontent.com/FFm412PQno8wQe6dMTs5ntsd3egS58ysEkHGqyDrMC2DRa4NwfmYBN4nAutn1v8STh-hZhIjgRwYaGYqgLo4SOV5WeWPxkQEP9HKktzO56D-TIEKPOIT3Jt-s0FTG9JsLvDMXmcT=s0)**


#### Générer **les clés ssh**

Installation du **ssh**

`~$ sudo apt-get install ssh`

Lancer ssh


   `~$ sudo /etc/init.d/ssh start`

Vérification


   `~$ sudo /etc/init.d/ssh status`

Génération clefs privée/publique à faire sur le serveur Ansible :


   `~$ ssh-keygen -t ed25519`

><u>Pourquoi avons-nous choisi cette clé ssh? </u>
_Les clés ECC (ECDSA et Ed25519) sont plus petites mais aussi plus sécurisées que leurs ancêtres, ce qui prend moins de ressources pour chiffrer et déchiffrer._

* Choisir le répertoire proposé par défaut
* Définir une passphrase

Copier la clé publique vers le serveur Proxmox



* Se placer dans le répertoire **/home/username/.ssh** : 

   `~$ cat id_ed25519.pub`

* Copier la clé manuellement
* Aller dans serveur proxmox, se placer dans le répertoire 

   `~$ cd /.ssh `

* Coller la clé dans le fichier **authorized_keys** avec un éditeur de texte, ensuite faire un cat pour afficher la clé : 

    `~# cat /root/.ssh/authorized_keys`

* Revenir sur kali et sortir du répertoire **/.ssh** avec “cd”
* Se connecter au serveur via ssh : 

  `~$ ssh root@Adresse-IP-Serveur`

* En cas d'erreur ci-dessous, saisir la commande suivante :

**![](https://lh4.googleusercontent.com/KF39X_UlUF3NTGxuL14TKCG8vlzIiCGnjzrb60amXQoJuvXPwCx0h3mtLJ0i7zDMr9RMkH-oBS-ks3agmEJNkRGEcq6kpHR06N5IrGiMl3IM5JvEoyYxcM_VME-omg1-dsgMWj7g=s0)**

`~$ ssh-keygen -f “/home/username/.ssh/know_hosts” -R “Adresse_IP_Serveur”`

#### Editer le fichier de configuration invetory 


* Se placer dans le répertoire ***/ansible-proxmox/**. Dans notre cas, le chemin absolu est le suivant :

   `~$ cd /home/username/Downloads/ansible-proxmox/`

* Créer une copie du fichier inventory.dist

   `~$ cp inventory.dist inventory`

* Ouvrir avec un éditeur de texte (exemple : nano) le fichier inventory

> \# cp inventory.dist inventory and adapt !
> \# Adresse IP Proxmox
> 5.39.65.74:8006 ansible_user=root ansible_port=22


**Fichier inventory.dist**
 ![](https://lh3.googleusercontent.com/mfaTSxWqXBnS3liJyyAmsWsWUxen2K4x0iyLnl1vK83i329afNqJODdKXRVdJHvBLSKbGrbYPZW8DyShYEs7TU34IqJRxiD-Rogde-bFp-h587ntBO2dRHIv2p5oEeHSWh-AIq5V=s0)
 
**Fichier inventory**

![](https://lh4.googleusercontent.com/kBFSmohpEAKC1LuLzd8sbM3vSJuknx8R-gKObc3xBrAvWrYfa_t5ucwWwcGn85dJwmYsuDVtHm07NykALm37nW9vhfmJEitc_4RUz9x0RiQpciedlEDKmctmJZC9FO6oPSyRu16j=s0)

#### Editer le module main



* Se placer dans le répertoire ***/ansible-proxmox/**. Dans notre cas, le chemin absolu est le suivant :

   `~$ cd /home/username/Downloads/ansible-proxmox/`
* Créer une copie du fichier main.yml.dist

   `~$ cp main.yml.dist main.yml`

* Ouvrir avec un éditeur de texte (exemple : nano) le fichier main.yml

><pre>\---
>
> - hosts: all
>
> vars:
>
> 
>
>    # network role
>
>    interfaces:
>
>    - { interface: 'vmbr1', address: '177.177.1.1', network: '177.177.1.0', netmask: '24' }
>
>    - { interface: 'vmbr2', address: '177.177.2.1', network: '177.177.2.0', netmask: '24' }
>
>    dhcp:
>
>    - { interface: 'vmbr1', first: '177.177.1.100', last: '177.177.1.200',
>
>    network: '177.177.1.0', broadcast: '177.177.1.255', netmask: '255.255.255.0', gateway: '177.177.1.1' }
>
>    - { interface: 'vmbr2', first: '177.177.2.100', last: '177.177.2.200',
>
>    network: '177.177.2.0', broadcast: '177.177.1.255', netmask: '255.255.255.0', gateway: '177.177.2.1' }
>
>  roles:
>
>    - { role: base }
>
>    - { role: network }</pre>

**Fichier main.yml**
**![11](https://lh6.googleusercontent.com/LqgWT5ud4cr3q0NL35ZJMJatpQ9luY77KF4GV3LQnokEnUFpk4ws_2p2_zoDwOe1TcWDu5NyKJ1RnoFOgrSD9QSv_L4HZ8WjasXv5YasCQCz-ZkXqof6ut5KA_eve6wKPMCdqub3=s0)**

**Fichier main**
**![13](https://lh5.googleusercontent.com/xCjGXRjVo9QGEJCloAq1N80hDib9Ea7zWe5G16Nlk2fxUYfru7zIjso0U25W_k0XtbGRyIq3ZfW3cPoLhJ6_lTo1lD_2kUlLg8sq9p0DtxikL5Acm5QEbxYCdm_fX74nzJk1kT6V=s0)**

**A partir de maintenant, Ansible peut exécuter des actions sur les serveurs cibles.**



* Se reconnecter au serveur via ssh :
`~$ ssh root@Adresse-IP-Serveur`
* Se placer dans ***/ansible-proxmox/**
* Lancer la commande ansible-playbook qui permettra l'exécution du module main : 

   `~$ ansible-playbook main.yml`


**![](https://lh4.googleusercontent.com/R1oCShbLmq0jDuFJ9waxKLN4hzZggtW0gd4ab6HWgBjdlsWLfVylFMMEHyT-N7fK2ZUkZmYCwmu9VpAB5tmwfLM-tTSFxjjFH9EMeJEpHFCCHNwgAgMB5ZxxaBWBK730sVK6j7wM=s0)**

**Résultat sur Proxmox : Système/Réseau**

**![](https://lh5.googleusercontent.com/R7Kzp8dEnpWBwvXSjxDjfxiaiMmqJEh6UQlnsnK49BfcoPqbfc2R_QFLpf7kS8G3SK73FrPa0tK71Zrw3NjAJbKhuMhdD_rxRK6CmXtIATY5btPqsVvBUFfBHdfwSneQnbxaRy9c=s0)**

# Conclusion

Nous avons travaillé sur ce brief dans l'objectif de configurer Ansible depuis Kali sur un serveur Proxmox qui joue le rôle d'un bac à sable.
Nous avons exploré ce logiciel libre qui permet le déploiement de manière automatique de tâches d’administrations vers plusieurs serveurs distant en même temps, quelque soit leur système d’exploitation.
Nous ne nous sommes pas contenté d'installer et de déployer Ansible-Proxmox mais également d'aller chercher plus loin notamment pour la création de l'environnement virtuel dont le but d'isoler le Node Manager par rapport à notre VM Kali.

Finalement, si nous avons à retenir le principe de déploiement Ansible, nous allons listé les points les plus importants:
* Définir le Node et le Node Manager
* Créer et installation d'Ansible
* Générer des clés ssh
* Editer le fichier de configuration inventory
* Editer le module main
* Lancer la commande qui permet d'exécuter les tâches : ansible-playbook
