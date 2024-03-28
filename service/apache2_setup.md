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
sudo apt install php8.1 php8.1-cli php8.1-common php8.1-curl php8.1-fpm php8.1-gd php8.1-http php8.1-igbinary php8.1-mbstring php8.1-mongodb php8.1-mysql php8.1-opcache php8.1-readline php8.1-redis php8.1-xml php8.1-zip php8.1-raphf
```

## APACHE INSTALL

```
# install from apt
sudo apt update
sudo apt install apache2 libapache2-mod-php8.1

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
- /etc/apache2/sites-available/example.com.conf

```
<VirtualHost *:80>

	# Domain Configuration
	ServerAdmin admin@example.com
	ServerName example.com
	#ServerAlias www.example.com

	# Directory Configuration
	DocumentRoot /var/www/test
	<Directory /var/www/test>
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
	<IfModule mod_dir.c>
		DirectoryIndex index.php index.html
	</IfModule>
	
	# Log Configuration
	ErrorLog ${APACHE_LOG_DIR}/example.com.http.error.log
	CustomLog ${APACHE_LOG_DIR}/example.com.http.access.log combined

</VirtualHost>
```
```
<VirtualHost *:443>

	# Domain Configuration
	ServerAdmin admin@example.com
	ServerName example.com
	#ServerAlias www.example.com

	# SSL Configuration
	SSLEngine on
	SSLCertificateFile /path/to/your/certificate.crt
	SSLCertificateKeyFile /path/to/your/private.key
	# SSLCertificateChainFile /path/to/your/chainfile.pem

	# Directory Configuration
	DocumentRoot /var/www/test
	<Directory /var/www/test>
		Options Indexes FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
	<IfModule mod_dir.c>
		DirectoryIndex index.php index.html
	</IfModule>

	# Log Configuration
	ErrorLog ${APACHE_LOG_DIR}/example.com.https.error.log
	CustomLog ${APACHE_LOG_DIR}/example.com.https.access.log combined

</VirtualHost>
```

## REVERSE PROXY CONFIG

- Enable required modules
```
sudo a2enmod proxy proxy_wstunnel proxy_http ssl rewrite
```

- VHOST config
```
<VirtualHost *:443>

	# Domain Configuration
	ServerName example.com
	# ServerAlias www.example.com
	# ServerAdmin admin@example.com

	# SSL Configuration
	SSLEngine on
	SSLProxyEngine on
	SSLCertificateFile /path/to/your/certificate.crt
	SSLCertificateKeyFile /path/to/your/private.key
	# SSLCertificateChainFile /path/to/your/chainfile.pem

	# Proxy Configuration
	ProxyRequests Off
    ProxyPreserveHost On
	ProxyPass / http://127.0.0.1:8080/
	ProxyPassReverse / http://127.0.0.1:8080/
	<Proxy *>
		Require all granted
	</Proxy>

	# Log Configuration
	ErrorLog ${APACHE_LOG_DIR}/example.com.https.error.log
	CustomLog ${APACHE_LOG_DIR}/example.com.https.access.log combined

</VirtualHost>
```