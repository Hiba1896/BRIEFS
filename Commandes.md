## lien symbolique
`ln -s cible lien`
*exemple*
`ln -s /mnt/disk2 /home/bob/disk2` crée un lien symbolique vers le répertoire disk2. le lien se voit dans le /home de bob
`ln -s /mnt/disk2/info.txt /home/bob/info.txt` crée un lien symbolique vers le fichier  info.txt, il est maintenant visible dans /home/bob/

le fichier info.txt est le même dans l'intégralité des répertoires où il est visible.
on peux le modifier dans tout les répertoires où il est visible.
Physiquement il est toujours au même endroit, il n'as pas été copié juste lisible dans différents répertoires.
Si il est modifié d'un manière ou d'une, peu importe depuis quel répertoire où il est lu la modification sera prise en compte car on as effectuer un lien vers info.txt présent dans /mnt/disk2
## cron
vi /etc/crontab
## logs
| *blank*     | commandes                     |
| ----------- | ----------------------------- |
| emplacement | /var/log/ <service>           |
|             | /syslog                       |
| méthodes    | `# grep DHCP /var/log/syslog` |
|             | `# tail -f /var/log/syslog`   |
|             | `journalctl -xe`              |