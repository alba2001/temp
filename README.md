##### Монтируем CD-ROM в Ubuntu
>mount /dev/cdrom /mnt/cdrom     
     
#####[Run PHP as User](http://helixdevelopment.com/dl/rau/current/doc/#instreq)
#####[how to enable mod_rewrite in apache2.2 (debian/ubuntu)](http://www.lavluda.com/2007/07/15/how-to-enable-mod_rewrite-in-apache22-debian/)
#####[Ubuntu - Nginx виртуальные](http://linux-bsd.in.ua/nginx/163-ubuntu-nginx-virtual-hosts.html)
#####[Настройка Nginx как front-end + Apache как back-end. Для чайников.](http://freehabr.ru/blog/linux/2365.html)
#####[Кэширующий прозрачный прокси сервер SQUID на Debian squeeze](http://habrahabr.ru/sandbox/39160/)#####
#####[Initial Server Setup with Ubuntu 12.04](https://www.digitalocean.com/community/articles/initial-server-setup-with-ubuntu-12-04)#####
#####[Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)#####
#####[Основные команды Git](http://crazycode.net/blog/5-versioning/8-git-main-commands)#####
#####[How to Launch Your Site on a New Ubuntu 12.04 Server with LAMP, FTP, and DNS](https://www.digitalocean.com/community/articles/how-to-launch-your-site-on-a-new-ubuntu-12-04-server-with-lamp-ftp-and-dns)#####
#####Пользуюсь Git, иногда при первом push в репозиторий можно встретить следующее сообщение об ошибке:#####
>! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to ...
для устранения этого, идем на сервер и в папке проекта пишем следующую комманду:
    
>git config receive.denyCurrentBranch ignore
    
#####Копирование по FTP#####
>wget -r -nH --cut-dirs=5 -nc ftp://user:pass@server//absolute/path/to/directory
       
>
* -nH avoids the creation of a directory named after the server name
* -nc avoids creating a new file if it already exists on the destination (it is just skipped)
* --cut-dirs=5 allows me to take the content of /absolute/path/to/directory and to put it in the directory where I launch wget. The number 5 is used to filter out the 5 components of the path. The double slash means an extra componen
    
#####Создать пользователя#####
>sudo useradd -d /home/testuser -s /bin/bash -m -c "Comment" testuser
    
##### Установка роутера    
> eth0-------------Internet    
> eth1-------------Internal        
    
* Configure the Internal interface (eth2) with static IP.    
> Edit the /etc/network/interfaces file and add following

 iface eth1 inet static
 address 192.168.0.1
 netmask 255.255.255.0
 network 192.168.0.0
 broadcast 192.168.0.255
 gateway 192.168.0.1
    
* Restart network and verify the eth2 interface's IP    
> \#/etc/init.d/network restart    
* Enable forwarding    
> \# cat /proc/sys/net/ipv4/ip_forward
0
> \# echo 1 > /proc/sys/net/ipv4/ip_forward
> or open the file manually and uncomment
> \# nano /etc/sysctl.conf
net.ipv4.ip_forward = 1    
* Add IPTABLES rule for NAT     
> \# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE    
* Final Testing    
> ping www.google.com        
    
#####Создание виртуального WEB сервера    
>sudo mkdir -p /home/artem/example.com/public_html    
>sudo chown -R artem:artem /home/artem/example.com/public_html     
>sudo chmod -R 755 /home/artem    
>sudo nano /home/artem/example.com/public_html/index.html    
>sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/example.com    
>sudo nano /etc/apache2/sites-available/example.com    
* DocumentRoot /home/artem/example.com/public_html    
    
>sudo a2ensite example.com
    
#####Красивый git log
>git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit    

#####[How to Set Up vsftpd on Ubuntu 12.04](https://www.digitalocean.com/community/articles/how-to-set-up-vsftpd-on-ubuntu-12-04)#####
    
В putty неверно отображается псевдографика Midnight Commander
------------------------------------------------------------
http://www.gentoo.ru/content/v-putty-neverno-otobrazhaetsya-psevdografika-midnight-commander

Если вместо рамок в mc отрисовываются различные символы, то измените настройки putty:
   
* Terminal > Keyboard > "The Function keys and keypad" = linux
* Window > Translation > Character set - выставляем правильную кодировку
* Connection > Data > "Terminal-type string" пишем linux
    
Сохраняем сессию, и после подключения mc будет отображать псевдографику корректно.

Ubuntu 12.04 git server
-----------------------
http://www.vxbus.com/software/linux/162-ubuntu-1204-git-server.html

1. install git:
>sudo apt-get install git-core git-doc
    
2. create gitolite user:
>sudo addgroup gitolite    
>sudo adduser –disabled-password –home /home/gitolite –ingroup gitolite gitolite
    
3. install gitolite:
>sudo apt-get -y install gitolite
    
4. setup gitolite:
>sudo usermod -a -G gitolite www-data    
>sudo service apache2 restart    
>sudo su – gitolite    
>gl-setup /tmp/your-username-goes-here.pub    
     
5. edit .gitolite.rc:
>$REPO_UMASK = 0027
    
6. manage gitolite:
>
\# FROM YOUR LOCAL MACHINE    
git clone gitolite@git.server:gitolite-admin.git
>
\# FROM YOUR LOCAL MACHINE    
cd gitolite-admin
emacs conf/gitolite.conf
>
\# change to:    
repo testing
RW+ = @all
R = daemon
testing “Owner” = “Test repo”
>
git add conf/gitolite.conf
git commit -m “Enabled gitweb and git-daemon export for testing repo”
git push
        
7. install gitweb:
>sudo apt-get install highlight gitweb    
>sudo emacs /etc/gitweb.conf    
>
\# change $projectroot to /home/gitolite/repositories    
\# change $projects_list to /home/gitolite/projects.list    
      
8. add user:
>
put "account-name.put" into "gitolite-admin/keypair"    
edit "conf/gitolite.conf", change to    
@developer root, account-name    

Setting bash as default shell
-----------------------------
>chsh -s /path/to/shell {user-name}    
то есть    
>chsh -s /bin/bash user    
