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
### Create a config file to override ssh settings
```/etc/ssh/sshd_config.d/99-hardened.conf```
```
PasswordAuthentication no
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
if its good restart the service
```
sudo systemctl restart sshd
```
