# NETWORK STORAGE

## UBUNTU

- **install nfs package**  
sudo apt install nfs-common
```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  keyutils libnfsidmap1 rpcbind
Suggested packages:
  watchdog
The following NEW packages will be installed:
  keyutils libnfsidmap1 nfs-common rpcbind
0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
Need to get 381 kB of archives.
After this operation, 1,447 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
...
```
- **create a mount point**  
sudo mkdir -p /red_nas
```
sudo ls -la /
total 4194384
drwxr-xr-x  20 root root       4096 Aug 30 20:01 .
drwxr-xr-x  20 root root       4096 Aug 30 20:01 ..
...
drwxr-xr-x   2 root root       4096 Aug 30 20:01 red_nas
...
```
- **mount nfs path**  
sudo mount -t nfs 10.10.10.10:/volume1/nuc_server /red_nas
```
Created symlink /run/systemd/system/remote-fs.target.wants/rpc-statd.service/lib/systemd/system/rpc-statd.service.
root@rednucserver:/red_nas# df -h
Filesystem                         Size  Used Avail Use% Mounted on
tmpfs                              784M  1.5M  783M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  231G  9.7G  210G   5% /
tmpfs                              3.9G     0  3.9G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
/dev/nvme0n1p2                     2.0G  128M  1.7G   7% /boot
/dev/nvme0n1p1                     1.1G  5.3M  1.1G   1% /boot/efi
tmpfs                              784M  4.0K  784M   1% /run/user/1000
10.10.10.10:/volume1/nuc_server    5.3T  437G  4.9T   9% /red_nas
```
- \*Unmounting nfs, just in case you dont need it anymore\*  
sudo umount /red_nas

- **add nfs mount on boot**  
sudo vim /etc/fstab
```
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
...
10.10.10.10:/volume1/nuc_server /red_nas    nfs defaults    0   0
...
```

# DISK SHARE(FOR WIN)
- Install Samba
```
sudo apt install samba -y
...
red@data1066:~$ whereis samba
samba: /usr/sbin/samba /usr/lib/x86_64-linux-gnu/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz

red@data1066:~$ samba -V
Version 4.15.13-Ubuntu

red@data1066:~$ systemctl status smbd
● smbd.service - Samba SMB Daemon
     Loaded: loaded (/lib/systemd/system/smbd.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2023-03-28 17:50:53 UTC; 2min 45s ago
       Docs: man:smbd(8)
...
```
- Config Samba
```
sudo vim /etc/samba/smb.conf

workgroup = WORKGROUP
server string = data1066
interfaces = lo enp5s0
bind interfaces only = yes

[netdisk1]
    comment = DATA1066-NETDISK1
    path = /netdisk1
    browsable = yes
    writable = yes
    guest ok = yes
    read only = no
    create mask = 0755
;   valid users = @red @root


red@data1066:~$ testparm
Load smb config files from /etc/samba/smb.conf
Loaded services file OK.
Server role: ROLE_STANDALONE
```
- Restart Service
```
sudo chown nobody:nogroup /netdisk1
sudo systemctl restart smbd.service nmbd.service
```
