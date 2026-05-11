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
## Create and Add your SSH keys to the VPS
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
```
ssh-copy-id -i ~/.ssh/keyname.pub yourusername@your-vps-ip
```
