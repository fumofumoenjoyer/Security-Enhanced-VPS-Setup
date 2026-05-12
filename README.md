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
Disable cockpit
```
sudo firewall-cmd --permanent --remove-service=cockpit
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

### Create a config file to override ssh settings
```/etc/ssh/sshd_config.d/99-hardened.conf```
```
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 0
AllowUsers yourusername
```
Check the config (if it returns nothing its good) 
```
sudo sshd -t
```
If everything is good restart the service
```
sudo systemctl restart sshd
```
## Install fail2ban
```
sudo dnf install fail2ban -y
```
```/etc/fail2ban/jail.local```
```
[sshd]
enabled = true
port = 22
maxretry = 3
bantime = 3600
findtime = 600
backend = systemd
```
```
sudo systemctl enable --now fail2ban
sudo fail2ban-client status sshd
```
