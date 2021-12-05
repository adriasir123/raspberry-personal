# Specs

Raspberry Pi 4 Model B



# First steps

Use [Raspberry Imager](https://www.raspberrypi.com/software/) to prepare your SD card.  
In Ubuntu, you can find it at the Software Store as a snap package.

Usage:

1. Select your operating system
2. Select media to write (SD card)



# Access info

- OS: Ubuntu 20.04.3 LTS
- User: sisyphus
- Password: zEBqw389
- Hostname: tartarus
- eth0 (static IP router DHCP reservation): 192.168.1.200/24
- wlan0 (static IP router DHCP reservation): 192.168.1.201/24



# Software

Slow (not recommended)

- Jellyfin 
- Nextcloud
- Modded minecraft server

## vsftpd

https://phoenixnap.com/kb/install-ftp-server-on-ubuntu-vsftpd

Install and enable:
```
sudo apt install vsftpd
sudo systemctl enable vsftpd
```

Backup config:
```
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.backup
```

Create FTP user:
```
sudo useradd -m mama
sudo passwd mama
```
Password: mama

Change HOME_DIR:
```
sudo usermod -d /home/sisyphus/external-hdd/media-libraries/mama mama
```

Modify `/etc/vsftpd.conf`:
```
listen=YES
listen_ipv6=NO
connect_from_port_20=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=45000
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
```

Modify `/etc/vsftpd.userlist`:
```
mama
```

Restart:
```
sudo systemctl restart vsftpd.service
```

# External HDD

Create mount point directory:
```
sudo mkdir external-hdd
```

Mount the partition to the directory:
```
sudo mount -t auto -v /dev/sda1 external-hdd
```






