# About

For some reason svn is still the freebie king to version control lots of binary files e.g. for a UE5 game.  
This is an attempt for an ez docker compose stack for svn over https (yes I know https performance will be ass, but easier than dealing with svn+ssh).  
It is intended to be reverse proxied by traefik (example in traefik dir, web network needs to be created before).  
Since Linux admin stuff isn’t exactly one of my strengths, don’t trust everything I’m doing here — if you spot a problem, just let me know or open an issue.

## iF.SVNAdmin Config

Available at /svnadmin  
Configure adminpanel ASAP after startup!

| Key                                    | Value                                |
| -------------------------------------- | ------------------------------------ |
| Subversion authorization file          | /etc/apache2/svn-auth/dav_svn.authz  |
| User authentication file (SVNUserFile) | /etc/apache2/svn-auth/dav_svn.passwd |
| Parent directory of the repositories   | /var/svn                             |
| Subversion client executable           | /usr/bin/svn                         |
| Subversion admin executable            | /usr/bin/svnadmin                    |

DO NOT FORGET TO ASSIGN A NEW PASSWORD AFTER INITIAL LOGIN!!!

### More configuration

It seems some settings like repo delete can only be changed by editing the config.ini

```bash
# stop svn before attaching to other container
sudo docker compose down
# mount in other container to edit from there
sudo docker run --rm -it -v svn-apache2-webdav-docker-compose_svnadmin_data:/data ubuntu bash
# within ubuntu container
apt update && apt install nano && nano /data/config.ini

```

## Shoutouts

Admin gui: [iF.SVNAdmin](https://github.com/mfreiholz/iF.SVNAdmin)  
This repo is loosely based on [elleFlorio/svn-docker](https://github.com/elleFlorio/svn-docker) which has been discontinued (sadly).
