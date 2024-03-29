# Specs

Raspberry Pi 4 Model B



# First steps

Use [Raspberry Imager](https://www.raspberrypi.com/software/) to prepare your SD card.  
In Ubuntu, you can find it at the Software Store as a snap package.

Usage:

1. Select your operating system
2. Select media to write (SD card)



# Access info

- OS: Ubuntu Server 20.04.5 LTS
- User: sisyphus
- Password: zEBqw389
- Hostname: tartarus
- eth0 (static IP router DHCP reservation): 192.168.1.200/24
- wlan0 (static IP router DHCP reservation): 192.168.1.201/24



# Software

Not recommended (slow or other reasons)

- Jellyfin 
- Nextcloud
- Modded minecraft server
- UMS

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

Create FTP users:

```
sudo useradd -m mama
sudo passwd mama
sudo useradd -m adrian
sudo passwd adrian
```

Passwords: mama, adrian

Change HOME_DIRs:

```
sudo usermod -d /home/sisyphus/external-hdd/media-libraries/mama mama
sudo usermod -d /home/sisyphus/external-hdd/media-libraries/adrian adrian
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
adrian
```

Restart:

```
sudo systemctl restart vsftpd.service
```



## MiniDLNA

Requirements:
```
sudo apt install minidlna
```

Modify these lines in `/etc/minidlna.conf`:
```
media_dir=/home/sisyphus/external-hdd/media-libraries
friendly_name=tartarus-DLNA
```

Restart:
```
sudo systemctl restart minidlna.service
```



## Plex 

Repository:

```
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
```

Update and install:

```
sudo apt update
sudo apt install plexmediaserver
```

Verify that Plex is running:

```
sudo systemctl status plexmediaserver
```

Set plex owner in libraries:

```
sudo chown -R plex: /home/ubuntu/external-hdd/media-libraries
```

Access Plex on: http://192.168.1.200:32400/web



# External HDD

Create mount point directory:

```
sudo mkdir external-hdd
```

## Non-persistent quick mount

Mount the partition to the directory:
```
sudo mount -t auto -v /dev/sda1 external-hdd
```

## Persistent mount

Find the UUID of the partition:

```
sudo blkid /dev/sda1
```

Backup fstab:
```
sudo cp /etc/fstab /etc/fstab.bkp
```

Add the next line to the end of fstab:
```
UUID=68967822-f8be-494a-98f4-cc1f953f6b6e /home/ubuntu/external-hdd       ext4    noatime,x-systemd.automount,x-systemd.device-timeout=10,x-systemd.idle-timeout=1min 0 2
```

Make the changes in fstab work:
```
sudo mount -a
```

Reboot to make sure the changes worked:
```
sudo reboot
```
