temp
====

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
>
sudo usermod -a -G gitolite www-data
sudo service apache2 restart
sudo su – gitolite
gl-setup /tmp/your-username-goes-here.pub
     
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
sudo emacs /etc/gitweb.conf
>
\# change $projectroot to /home/gitolite/repositories    
\# change $projects_list to /home/gitolite/projects.list    
      
8. add user:
>
put "account-name.put" into "gitolite-admin/keypair"
edit "conf/gitolite.conf", change to
@developer root, account-name