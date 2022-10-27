# Ubuntu
## User
* add user
~~~
sudo adduser sam

Adding user `sam' ...
Adding new group `sam' (1002) ...
Adding new user `sam' (1001) with group `sam' ...
Creating home directory `/home/sam' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for sam
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] Y
red@rednucserver:~/app$
~~~
* change password
~~~
sudo passwd sam

New password:
Retype new password:
passwd: password updated successfully
~~~
* delete user
~~~
sudo deluser sam

sudo deluser --remove-home sam
~~~
## Group
~~~
sudo addgroup poweruser

Adding group `poweruser' (GID 1003) ...
Done.
~~~
~~~
sudo adduser sam poweruser

Adding user `sam' to group `poweruser' ...
Adding user sam to group poweruser
Done.
~~~
~~~
sudo deluser sam poweruser

Removing user `sam' from group `poweruser' ...
Done.
~~~
~~~
sudo delgroup poweruser

Removing group `poweruser' ...
Done.
~~~

## OPENSSH CONFIG
* set poweruser group can sudo without password
~~~
sudo vim /etc/sudoers

...
# Allow members of group poweruser to sudo without password
%poweruser    ALL=(ALL) NOPASSWD: ALL
...
~~~
* allow password login and disable root
~~~
sudo vim /etc/ssh/sshd_config

...
PermitRootLogin no
...

...
PasswordAuthentication yes
...

~~~
* restart ssh service
~~~
sudo systemctl restart ssh
~~~

* connect from remote with rsa-key
~~~
vim ~/.ssh/authorized_keys

ssh-rsa AAAA...{YOUR PUBLIC KEY HERE}...B== xxx_x13
~~~

* connect to remote with rsa-key
~~~
ssh-keygen -t rsa -b 4096 -C "xxx@gmail.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/home/xxx/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/xxx/.ssh/id_rsa
Your public key has been saved in /home/xxx/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xzl4s1H0eN*****kgG1Uw+/hMvHUhT2k xxx@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|  +o.       o   .|
| . o   0 . + = ..|
|*****************|
|*****************|
|   + o . + *   o |
|    0     .   .  |
|   * O       .   |
|       *         |
+----[SHA256]-----+

cat /home/red/.ssh/id_rsa.pub

ssh-rsa AAA...{YOUR PUBLIC KEY HERE}...nw== xxx@gmail.com
~~~
