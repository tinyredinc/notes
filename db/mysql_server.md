# UBUNTU

- refresh the package repo  
sudo apt update  
sudo apt upgrade

- install via apt  
sudo apt list mysql*
```
Listing... Done
mysql-client-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-client-core-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-client/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 all
mysql-common/jammy 5.8+1.0.8 all
mysql-router/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-sandbox/jammy 3.2.05-1 all
mysql-server-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-server-core-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-server/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 all
mysql-source-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-testsuite-8.0/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 amd64
mysql-testsuite/jammy-updates,jammy-security 8.0.30-0ubuntu0.22.04.1 all
mysqltcl/jammy 3.052-3ubuntu1 amd64
mysqltuner/jammy 1.7.17-1 all
```
- install mysql server 8.0.x  
sudo apt install mysql-server  

- instal mysql client 8.0.x  
sudo apt install mysql-client

```
sudo systemctl start mysql.service
mysql --version
mysql  Ver 8.0.30-0ubuntu0.22.04.1 for Linux on x86_64 ((Ubuntu))
```
- set a password for root  
sudo mysql
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'strong_password';
Query OK, 0 rows affected (0.01 sec)

mysql> exit
```

- init the installation  
sudo mysql_secure_installation
```
red@rednucserver:~$ sudo mysql_secure_installation

Securing the MySQL server deployment.

Enter password for user root:
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```

- create a dba account for daily remote connection
```
mysql -u root -p

mysql> CREATE USER 'dev_master'@'%' IDENTIFIED BY 'strong_password';
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT ALL ON *.* TO 'dev_master'@'%' WITH GRANT OPTION;
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
```

- tuning config file  
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
...
# bind address to eth static ip
bind-address            = 10.10.10.20
...
```
```
#######################
# innodb quick tuning #
#######################

# innodb_buffer_pool_size 
# Buffer Pool is basically memory area where InnoDB stores tables data and index cache. Increasing buffer pool is always a good step to start tuning from. Set buffer pool size to 80% of available RAM on your server.
innodb_buffer_pool_size = 4G

# innodb_log_file_size
# When data is changes in InnoDB table, Mysql will write changes to buffer pool first. After that it will log change operations (in a low-level format) to a log file (so called “redo log”). If those log files are small, Mysql have to make a lot of writes to disk directly to data files. So set log files large (as big as buffer pool, if possible).
innodb_log_file_size = 4G
# warning, when you change innodb_log_file_size, you have to remove old log files and restart mysql:
# sudo systemctl stop mysql
# sudo rm /var/lib/mysql/ib_logfile*
# sudo systemctl start mysql

# innodb_log_buffer_size
# If log buffer is smaller than the transaction impact, Mysql will have to wait till it writes everything to disk. Increase log buffers if you have queries that insert/update/delete many rows at once.
innodb_log_buffer_size = 128M

# innodb_flush_log_at_trx_commit
# default value of 1 will flush data to disk after each transaction. Setting innodb_flush_log_at_trx_commit to 0 will ask Mysql to flush data to disk every second. This will improve write performance dramatically. But be careful. This can lead to up to 1 second of lost transactions on system crashes
innodb_flush_log_at_trx_commit = 0

# innodb_flush_method
# [fsync] uses the fsync() system call to flush both the data and log files; [O_DSYNC] uses O_SYNC to open and flush the log files, and fsync() to flush the data files. Long story short, fsync = compatible and O_DSYNC = fast
innodb_flush_method = O_DSYNC
```