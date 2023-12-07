# UBUNTU

## COMMON COMMAND

```
# enable site config
sudo a2ensite your_domain.conf

# disable site config
sudo a2dissite your_domain.conf

# test config
sudo apache2ctl configtest

# reload config
sudo systemctl reload apache2

# restart service
sudo systemctl restart apache2
```

## PHP INSTALL

```
sudo apt install php8.1 php8.1-cli php8.1-common php8.1-curl php8.1-fpm php8.1-gd php8.1-http php8.1-igbinary php8.1-mbstring php8.1-mongodb php8.1-mysql php8.1-opcache php8.1-readline php8.1-redis php8.1-xml php8.1-zip
```

## APACHE INSTALL

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

## VHOST CONFIG

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