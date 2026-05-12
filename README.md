# Security-Enhanced-VPS-Setup
Security Enhanced VPS Setup (SEVPSS)
## Log into your fresh VPS as root
Change the IP for the one you have
```
ssh root@your-vps-ip
```
## Create an user with sudo privileges
Replace fumo with the name of the user you want
```
useradd yourusername
passwd yourusername
usermod -a -G wheel,users,yourusername yourusername
```
## SSH Config
### Create and Add your SSH keys to the VPS
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
```
ssh-copy-id -i ~/.ssh/keyname.pub yourusername@your-vps-ip
```
### Log into your vps with the key
```
ssh -i /home/fumo/.ssh/keyname 'yourusername@your-vps-ip'  
```
### Update and install stuff
This is what i usually install on mine but you definitely need ```firewalld```
```
sudo dnf update -y
sudo dnf install epel-release firewalld git waypipe -y
```
enable the firewall
```
sudo systemctl enable --now firewalld
```

### Create a config file to override ssh settings
```/etc/ssh/sshd_config.d/99-hardened.conf``` (change the port if you want)
```
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
Port 49152
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 0
AllowUsers yourusername
```
Check the config (if it returns nothing its good) 
```
sudo sshd -t
```
Tell SELinux about the new port
```
semanage port -l | grep ssh
```
```
sudo semanage port -a -t ssh_port_t -p tcp 49152
```
(Note: If ```semanage``` is not found, install it via ```sudo dnf install policycoreutils-python-utils```)
add the port to the firewall
```
sudo firewall-cmd --permanent --add-port=49152/tcp
sudo firewall-cmd --reload
```

If everything is good restart the service
```
sudo systemctl restart sshd
```
