# UBUNTU

## INSTALL

```
# install from apt
sudo apt update
sudo apt install apache2

# set to start at boot
sudo systemctl enable apache2

# start apache2 service
sudo systemctl start apache2
sudo systemctl status apache2

#restart service
sudo systemctl restart apache2
```

## CONFIG
- EXAMPLE HTTP/HTTPS VHOST CONFIG
- /etc/apache2/sites-available/fs.sideai.com.conf
```
<VirtualHost *:80>
	ServerAdmin admin@sideai.com
	ServerName fs.sideai.com
	#ServerAlias www.example.com
	DocumentRoot /var/www/test
	<Directory /var/www/test>
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/fs.sideai.com.error.log
	CustomLog ${APACHE_LOG_DIR}/fs.sideai.com.access.log combined
	<IfModule mod_dir.c>
		DirectoryIndex index.php index.html
	</IfModule>
</VirtualHost>
```
```
<VirtualHost *:443>
	SSLEngine on
	SSLCertificateKeyFile /var/ssl/key/wildcard.sideai.com.key
	SSLCertificateFile /var/ssl/cert/wildcard.sideai.com.231027.crt
	ServerAdmin admin@sideai.com
	ServerName fs.sideai.com
	#ServerAlias www.example.com
	DocumentRoot /var/www/test
	<Directory /var/www/test>
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/fs.sideai.com.https.error.log
	CustomLog ${APACHE_LOG_DIR}/fs.sideai.com.https.access.log combined
	<IfModule mod_dir.c>
		DirectoryIndex index.php index.html
	</IfModule>
</VirtualHost>
```
- CONFIG COMMAND
```
# enable site config
sudo a2ensite your_domain.conf

# disable site config
sudo a2dissite your_domain.conf

# test config
sudo apache2ctl configtest

# reload config
sudo systemctl reload apache2
```
