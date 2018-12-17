# Yiimp_install_scrypt (update October, 2018)


Discord : https://discord.gg/zcCXjkQ

TUTO Youtube (16.04 - Without SSL) : https://www.youtube.com/watch?v=vdBCw6_cyig

TUTO Youtube (16.04 - With SSL) : https://www.youtube.com/watch?v=fWwGow_i-Vw

Official Yiimp (used in this script for Yiimp Installation): https://github.com/tpruvot/yiimp

***********************************

## Install script for yiimp on Ubuntu Server 16.04

USE THIS SCRIPT ON FRESH INSTALL UBUNTU Server 16.04 !

Connect on your VPS =>
- adduser pool
- adduser pool sudo
- su - pool
- sudo apt-get -y install git
- git clone https://github.com/xavatar/yiimp_install_scrypt.git
- cd yiimp_install_scrypt/
- sudo bash install.sh (Do not run the script as root)
- sudo bash screen-scrypt.sh (in tuto youtube, i launch the scrypt with root... it does not matter)
- NOT MANDATORY => sudo bash screen-stratum.sh (CONFIGURE BEFORE START this script... add or remove algo you use).

###########################
POOL VIDEOS 
https://www.youtube.com/watch?v=NcMAqSdZKa0&t=1236s
https://www.youtube.com/watch?v=n3__bpARLkY
https://www.youtube.com/watch?v=__C4mDA4SEc
https://www.youtube.com/watch?v=XRdLY0K9Bs4
###########################
site.com/phpmyadmin -> LOGIN => yiimpfrontend => coins => delete everything
site.com/phpmyadmin -> LOGIN => yiimpfrontend => rawcoins => delete everything

sudo nano /var/web/serverconfig.php
  define('YAAMP_NOTIFY_NEW_COINS', false);
  define('YAAMP_CREATE_NEW_COINS', false);
  
###########################
FIREWALL SETUP UBUNTU 16.04
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04
https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server

PORTS - OPEN OR CLOSE
https://serverfault.com/questions/309052/check-if-port-is-open-or-closed-on-a-linux-server
https://askubuntu.com/questions/928191/how-to-open-a-closed-port-in-ubuntu
  
sudo ufw allow 22
sudo ufw allow 80
sudo iptables -A INPUT -p tcp --dport 31020 -m state --state NEW -j ACCEPT
sudo iptables -A INPUT -p udp --dport 31020 -m state --state NEW -j ACCEPT

sudo iptables -A INPUT -p tcp --dport 30020 -m state --state NEW -j ACCEPT
sudo iptables -A INPUT -p udp --dport 30020 -m state --state NEW -j ACCEPT

sudo iptables -A INPUT -p tcp --dport 3739 -m state --state NEW -j ACCEPT
sudo iptables -A INPUT -p udp --dport 3739 -m state --state NEW -j ACCEPT

sudo iptables -A OUTPUT -p tcp --dport 3739 -m state --state NEW -j ACCEPT
sudo iptables -A OUTPUT -p udp --dport 3739 -m state --state NEW -j ACCEPT

sudo iptables -A OUTPUT -p tcp --dport 31020 -m state --state NEW -j ACCEPT
sudo iptables -A OUTPUT -p udp --dport 31020 -m state --state NEW -j ACCEPT

sudo iptables -A OUTPUT -p tcp --dport 30020 -m state --state NEW -j ACCEPT
sudo iptables -A OUTPUT -p udp --dport 30020 -m state --state NEW -j ACCEPT

###########################
DAEMON SERVER
###########################
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils

sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev

sudo apt-get install libminiupnpc-dev
sudo apt-get install libboost-all-dev

copy stratum folder to every wallet
add ip instead of 127.0.0.1/0 in external wallet

##########################
sudo apt-get update
sudo apt-get install aptitude

##########################
mysql -h 127.0.0.1 -P 3306 -u root -p <database>
Also (to see if it's running):

telnet 127.0.0.1 3306 

MYSQL - port 3306
https://www.digitalocean.com/community/tutorials/how-to-backup-mysql-databases-on-an-ubuntu-vps
https://www.configserverfirewall.com/ubuntu-linux/enable-mysql-remote-access-ubuntu/
https://www.techrepublic.com/article/how-to-set-up-mysql-for-remote-access-on-ubuntu-server-16-04/
sudo ufw allow 3306
sudo iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

INSTALL PHPMYADMIN
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04

###########################

Finish !
Go http://xxx.xxxxxx.xxx or https://xxx.xxxxxx.xxx (if you have chosen LetsEncrypt SSL). Enjoy !

###### :bangbang: **YOU MUST UPDATE THE FOLLOWING FILES :**
- **/var/web/serverconfig.php :** update this file to include your public ip (line = YAAMP_ADMIN_IP) to access the admin panel (Put your PERSONNAL IP, NOT IP of your VPS). update with public keys from exchanges. update with other information specific to your server..
- **/etc/yiimp/keys.php :** update with secrect keys from the exchanges (not mandatory)


###### :bangbang: **IMPORTANT** : 

- The configuration of yiimp and coin require a minimum of knowledge in linux
- Your mysql information (login/Password) is saved in **~/.my.cnf**
- **If you reboot your VPS**, you must restart screen-scrypt.sh (or add crontab)
- Remember to restart **memcached service** after the db change (update or import new .sql)

***********************************

###### This script has an interactive beginning and will ask for the following information :

- Enter time zone
- Server Name 
- Are you using a subdomain
- Enter support email
- Set stratum to AutoExchange
- New location for /site/adminRights
- Your Public IP for admin access (Put your PERSONNAL IP, NOT IP of your VPS)
- Install Fail2ban
- Install UFW and configure ports
- Install LetsEncrypt SSL
