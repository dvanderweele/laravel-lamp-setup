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
Enable mod_rewrite in apache2:
```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```
Don't do this now, but if you wanted to disable that same apache2 mod:
```bash
sudo a2dismod rewrite
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
Within your home directory, get the Composer installer:
```bash
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php
```
From the [Composer Public Keys Page](https://composer.github.io/pubkeys.html) copy the hash and store it as a shell variable:
```bash
HASH=[actual hash copied from Composer website]
```
Run the followings cript to make sure the installation script is safe:
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
