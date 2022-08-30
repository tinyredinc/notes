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
