# Magento 2 Installation on Google Cloud VPS (Ubuntu 24.04 LTS)

Welcome to this detailed guide on installing Magento 2 on a Google Cloud Virtual Private Server (VPS) running Ubuntu 24.04 LTS. Whether you're launching a new e-commerce platform or migrating an existing store, this repository provides a step-by-step walkthrough to help you get started.

## Step 1: Update Ubuntu Repositories

```bash
sudo apt-get update
```
## Step 2: allow login to root user and set up password for root

```bash
sudo passwd root
```

## Step 3: Install Apache2
```bash
sudo apt-get install apache2 -y
```

## Step 4: edit "/etc/apache2/sites-available/000-default.conf"

add
```
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

## Step 5: edit "/etc/apache2/apache2.conf"

add
```
ServerName 34.89.22.193
```

Where 34.89.22.193 is IP address of server

## Step 6: check if there is no syntax errors

```bash
sudo apachectl configtest
```

## Step 7: Enable rewrite on the server
```bash
 sudo a2enmod rewrite
```

## Step 8: Restart apache
```bash
sudo systemctl restart apache2
```

## Step 9: Install PHP

```bash
sudo apt install php-{bcmath,common,curl,fpm,gd,intl,mbstring,mysql,soap,xml,xsl,zip,cli} libapache2-mod-php zip unzip -y
```

## Step 10: edit /etc/apache2/mods-enabled/dir.conf

Tell the web server to prefer PHP files (move index.php to front of the list)

## Step 11: Restart apache

```bash
sudo systemctl restart apache2
```

## Step 12: Install Mysql
```bash
sudo apt-get install mysql-server -y
```

# Step 13: Install phpmyadmin
```bash
sudo apt-get install gettext -y    
```

```bash
sudo apt-get install phpmyadmin php-mbstring -y
```

```bash
sudo systemctl restart apache2
```

```bash
sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
sudo a2enconf phpmyadmin.conf
sudo systemctl reload apache2.service
```

## Step 14: Set password for root user in mysql
```bash
 sudo mysql -u root -p
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
```

## Step 15: add user for magento
```bash
sudo adduser magento
```

```bash
sudo usermod -g www-data magento
```

## update permissions
```bash
sudo chown magento:www-data /var/www/html/
```

## Generate keys
public key is login private key is password
```
https://commercemarketplace.adobe.com/customer/accessKeys/
```

## Install composer
```bash
curl -sS https://getcomposer.org/installer | php
```

```bash
sudo mv composer.phar /usr/local/bin/composer
```

## Change directory
```bash
/var/www/html
```
##



