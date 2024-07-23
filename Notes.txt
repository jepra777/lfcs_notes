Notes1:

1. [Find] 
find /opt/findme -type f -executable > /opt/foundthem.txt --> to find with (x) permission
find /opt/findme -type f -perm /4000 -exec rm {}+  --> to find the SETUID file and remove them
find /opt/findme -type f -size +1k --> to find the file larger than 1kb

2. [Scripting]
#!/bin/bash

cp -a /var/www/. /opt/www-backup/

sudo chmod +x /opt/script.sh

sudo vi /etc/crontab --> System Wide 
0 4 * * * root /opt/script.sh

3. [Limits]
sudo vi /etc/security/limits.conf

4. [Create User]
useradd mary --shell /bin/dash
passwd 1234
usermod -aG sudo mary --> assign sudo as the 2nd group of user account mary
usermod -g developers mary --> assign developers as the 1st group of user account mary

5. [Kernel Runtime Parameter]
sudo sysctl -w vm.swappiness=10

vi /etc/sysctl.d/99-vm_swap.conf
vm.swappiness=10
sysctl -p /etc/sysctl.d/vm_swap.conf

6. [/etc/fstab]

/dev/vdb1 /backups xfs defaults 0 2 --> Mount Permanent

sudo mount -o remount,ro /dev/vdb2 /mnt --> Remount and Read-Only

sudo mkfs.xfs -f -L ExamFS /dev/vdb3 --> Format and Labeling 

7. [LVM]
pvcreate /dev/vdc /dev/vdd
vgcreate volume1 /dev/vdc /dev/vdd
lvcreate --size 3G --name website_files volume1

8. [Github]
git init
git remote add origin <URL>
git pull origin master

9.[Docker]
docker ps
docker build -t <image-name> .
docker run -d -p 8081:80 --name <container-name> <image-name>
docker rm
docker rmi

10. [NFS Server - Setting]
/etc/exports
/home	10.0.0.0/24(ro)
sudo exportfs -ra

11. [Check PID]
ss -tulpn | grep -i<specific-port>
pstree -p <pid>

12. [Assign IP]
ip address add <IP>/<CIDR> dev <interface>
cat /etc/resolv.conf, to see the DNS IP
ip address add default via <IP> dev <interface>

13. [Disk Partition]
cfdisk
mkfs.ext4
mkfs.xfs
/etc/fstab
mount <dev> <path>

14. [Swap]
sudo fallocate -l 1024M /swfile
sudo chmod 600 /swfile
sudo mkswap /swfile
sudo swapon /swfile

vi /etc/fstab:
/swfile none swap sw 0 0

15. [Troubleshot Process Disk]
iostat (Check the Disk Device)
pidstat -d (Check the PID Process)

16. [Find]
- Find the largest path: find * -size +4k 
- Or you can using: du -shc

17. [SSH]
Config for Specific Users
Example:
Match User bob
        X11Forwarding yes

---

Notes2:
1. [NTP]
/etc/system/timesyncd.conf

systemctl restart systemd-timesyncd

timedatectl show-timesync -a

timedatectl list-timezone

timedatectl set-timezone "Europe/Bucharest"

2. [CRON]
sudo crontab -eu john
0 4 * * 4 /usr/bin/find -type d -empty -delete

3. [Check IP Interface]
ip a
echo "eth1" > /opt/interface.txt

4. [User]
usermod -g jane jane
usermod -aG sudo jane

mkdir /home/jane
chown jane:jane /home/jane

usermod -s /bin/bash jane

5.[IPTABLES Redirects]

touch /etc/sysctl.d/10-ip-forward.conf
add:
net.ipv4.ip_forward=1
net.ipv6.conf.forwarding=0
sysctl -p /etc/sysctl.d/10-ip-forward.conf

iptables -t nat -A PREROUTING -p tcp -s 10.5.5.0/24 --dport 81 -j --to-destination 192.168.5.20
iptables -t nat -A POSTROUTING -s 10.5.5.0/24 -j MASQUERADE

apt install iptables-persists
iptables-save /etc/iptables/rules.v4

6. [OpenSSL]

openssl x509 -in file* -noout -text | grep -I "Public-Key"

7. [ACL]
setfacl -m u:janet:rw
getfacl

8. [Limits]
vi /etc/security/limits.conf

9. [Git]
touch file1
git add file1
git commit -m "Created first required file"
git push origin master

10. [Selinux]
getenforce > /opt/selinuxmode.txt
restorecon /usr/bin/less

11. [Service]
systemctl start nginx
systemctl enable nginx
systemctl status nginx
ps -aux | grep -inginx
echo "www-data" > /opt/nginxuser.txt

12. [LVM]
vgextend volume1 /dev/vdc
lvresize --size 2GB /dev/volume1/lv1

13. [Git]
apt install git
git clone <URL>
less README
apt install libncursesw5-dev autotools-dev autoconf automake build-essential
./autogen.sh && ./configure && make
make install

14. [Virsh]
virt-install \
--name mockexam2 \
--ram 1024 \
--vcpus=1 \
--os-variant=ubuntu22.04 \
--import \
--disk path=/var/lib/libvirt/images/ubuntu.img \
--noautoconsole

virsh autostart mockexam2

15. [Bridge Network]
vi /etc/netplan/*.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    eth1:
      dhcp4: true
    eth2:
      dhcp4: true
  bridges:
    bridge1:
      interfaces:
        - eth1
        - eth2
      dhcp4: true

16. [Docker]
docker run -d -p 80:80 --name apache_container --restart on-failure:3 httpd

17. [OpenLDAP]
vi /etc/nslcd.conf

vi /etc/nsswitch.conf

systemctl restart nslcd
