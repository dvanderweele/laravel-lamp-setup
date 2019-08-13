# Installing & Configuring Laravel & Prerequisites

Install a few dependencies after initial server setup:
```bash
sudo apt update
sudo apt install -y git curl wget zip unzip apache2 mysql-server php libapache2-mod-php php-mysql php7.2-common php7.2-cli php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-mbstring php7.2-bcmath php7.2-imap php7.2-xml php7.2-zip
```
Make sure apache2 is running and enabled:
```bash
sudo systemctl status apache2
```
Have your firewall allow traffic to Apache:
```bash
sudo ufw allow in "Apache Full"
```
Quickly backup two important configuration files for Apache before modifying them:
```bash
cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak
cp /etc/apache2/conf-enabled/security.conf /etc/apache2/conf-enabled/security.conf.bak
```
Enable mod_rewrite in apache2:
```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```
Don't do this now, but if you wanted to disable that same apache2 mod:
```bash
sudo a2dismod rewrite
```
Hide Apache Version and Operating system in response headers by editing /etc/apache2/conf-enabled/security.conf so it has the following two lines:
```
ServerSignature Off
ServerTokens Prod
```
Reload Apache:
```bash
sudo systemctl reload apache2
```
Disable directory listing by editing /etc/apache2/apache2.conf so the Directory /var/www/ section looks like so:
```
<Directory /var/www/>
	Options -Indexes FollowSymlinks
	AllowOverride None
	Require all granted
</Directory>
```
Secure apache with mod_security and then restart the server:
```bash
sudo apt install libapache2-mod-security2 -y
sudo systemctl restart apache2
```
Edit /etc/apache2/apache2.conf to limit the storage directory of your laravel app where images are uploaded to a max of 10 megabytes:
```
<Directory "/var/www/html/storage">
LimitRequestBody 10485760
</Directory>
```
Note that you can setup a separate directory in /var/www instead of the html directory, perhaps named after your_domain.

Disable TRACE HTTP Requests in apache by adding the following to the same file above:
```
TraceEnable off
```
Secure apache with mod_evasive and then restart the server:
```bash
sudo apt install libapache2-mod-evasive -y
sudo systemctl restart apache2
```
Secure Shared Memory by editing the /etc/fstab file and adding the following line to the very bottom of the file:
```
none /run/shm tmpfs defaults,ro 0 0
```
You have to secure the mysql server:
```bash
sudo mysql_secure_installation
```
Answer YES to all questions while securing mysql installation.
Next, try to connect to mysql with root password:
```bash
sudo mysql -u root -p
exit
```
You need to tell apache to prefer php files over other files:
```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```
In that file, **index.php** should have priority over other files like index.html and index.cgi.
Restart Apache:
```bash
sudo systemctl restart apache2
```
Test the correctness of apache configuraton:
```bash
sudo nano /var/www/html/info.php
```
Put this in that file:
```php
<?php
phpinfo();
?>
```
Then visit that file in your browser at http://your_server_ip/info.php

Finally, delete that info.php file.

Let's set up a Virtual Host for your site. First set ownership & permissions of the website directory with the $USER environment variable:
```bash
sudo chown -R $USER:$USER /var/www/your_domain
sudo chmod -R 755 /var/www/your_domain
```
Create a new apache site configuration file so apache can serve your site:
```bash
sudo nano /etc/apache2/sites-available/your_domain.conf
```
This is what the contents of that file should look like, making sure that ServerAdmin is a valid email the site administrator can access:
```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	ServerName your_domain
	ServerAlias www.your_domain
	DocumentRoot /var/www/your_domain
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Enable the site:
```bash
sudo a2ensite your_domain.conf
```
Disable the default site:
```bash
sudo a2dissite 000-default.conf
```
Test for config errors:
```bash
sudo apche2ctl configtest
```
Restart Apache:
```bash
sudo systemctl restart apache2
```
Let's use Let's Encrypt to get an SSL certficate. Install the Certbot software:
```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python-certbot-apache
```
Run certbot with the --apache plugin for your_domain:
```bash
sudo certbot --apache -d your_domain -d www.your_domain
```
It is suggested to ask during configuration to choose option 2: Redirect.

Let's do a dry run of the automated certificate renewal process:
```bash
sudo certbot renew --dry-run
```
Within your home directory, get the Composer installer:
```bash
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php
```
From the [Composer Public Keys Page](https://composer.github.io/pubkeys.html) copy the hash and store it as a shell variable:
```bash
HASH=[actual hash copied from Composer website]
```
Run the following script to make sure the installation script is safe:
```bash
php -r "if (has_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
If you get Installer corrupt then you have to redownload install script and make sure the hash you copied is correct. Then verify again as above.
Install composer globally:
```bash
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
Test composer installation:
```bash
composer
```
Create a new Laravel project in a directory called Taylor_Otwell_Is_Basically_God:
```bash
composer create-project --prefer-dist laravel/laravel Taylor_Otwell_Is_Basically_God
```
Create a database for the Laravel project:
```bash
sudo mysql -u root -p
```
Then, at the mysql prompt:
```sql
CREATE DATABASE laravel;
GRANT ALL ON laravel.* to 'laravel'@'localhost' IDENTIFIED BY 'secret' WITH GRANT OPTION;
FLUSH PRIVILEGES;
quit
```
Then in your environment file for your Laravel project:
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```
To state the obvious, your password doesn't literally need to be *secret*.



