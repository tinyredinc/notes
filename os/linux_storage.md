# NETWORK STORAGE

## MOUNT NFS NET DISK

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

## SHARE NETDISK SAMBA(FOR WIN)
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

[netdisk]
    comment = DATA1066-NETDISK1
    path = /netdisk1
    browsable = yes
    writable = yes
    guest ok = yes
    read only = no
    create mask = 0755
;   valid users = @red @root

;[netdisk] - Represents the directory name. This is the directory location Windows users see.
;comment - Serves as a shared directory description.
;path - This parameter specifies the shared directory location. The example uses a directory in /home, but users can also place the shared files under /samba.
;read only - This parameter allows users to modify the directory and add or change files when set to no.
;writeable - Grants read and write access when set to yes.
;browseable - This parameter allows other machines in the network to find the Samba server and Samba share when set to yes. Otherwise, users must know the exact Samba server name and type in the path to access the shared directory.
;guest ok - When set to no, this parameter disables guest access. Users need to enter a username and password to access the shared directory.
;valid users - Only the users mentioned have access to the Samba share.

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

## SOFTWARE RAID 1
- List Disks
```
lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0  63.3M  1 loop /snap/core20/1822
loop1    7:1    0 111.9M  1 loop /snap/lxd/24322
loop2    7:2    0  49.8M  1 loop /snap/snapd/18357
sda      8:0    0 476.9G  0 disk
├─sda1   8:1    0     1M  0 part
├─sda2   8:2    0 428.9G  0 part /
└─sda3   8:3    0    48G  0 part [SWAP]
sdb      8:16   0   1.8T  0 disk
└─sdb1   8:17   0   1.8T  0 part
sdc      8:32   0   1.8T  0 disk
└─sdc1   8:33   0   1.8T  0 part
```
- Create RAID
```
root@data1066:/home/red# sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 1953382464K
mdadm: automatically enabling write-intent bitmap on large array
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```
- Check RAID status
```
cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid1 sdc[1] sdb[0]
      1953382464 blocks super 1.2 [2/2] [UU]
      [>....................]  resync =  1.7% (35079616/1953382464) finish=264.5min speed=120832K/sec
      bitmap: 15/15 pages [60KB], 65536KB chunk

unused devices: <none>
```
- Create Partition
```
sudo mkfs.ext4 -F /dev/md0
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 488345616 4k blocks and 122093568 inodes
Filesystem UUID: a9322e49-4706-4cdf-b378-7ff1197af4bd
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
        102400000, 214990848

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```
- Mount RAID
```
sudo mkdir -p /netdisk
sudo mount /dev/md0 /netdisk
```
- Add RAID to Boot
```
sudo mdadm --detail --scan
ARRAY /dev/md0 metadata=1.2 name=data1066:0 UUID=5bce11ea:ced7ca7e:54713008:187545f8

sudo vim /etc/mdadm/mdadm.conf
...
ARRAY /dev/md0 metadata=1.2 name=data1066:0 UUID=5bce11ea:ced7ca7e:54713008:187545f8

sudo update-initramfs -u
update-initramfs: Generating /boot/initrd.img-5.15.0-69-generic
I: The initramfs will attempt to resume from /dev/sda3
I: (UUID=c454a1fa-6b00-4022-b75f-111d214a05ca)
I: Set the RESUME variable to override this.

sudo vim /etc/fstab
/dev/md0 /netdisk ext4 defaults,nofail,discard 0 0
```
