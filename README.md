# Magento 2 Installation on Google Cloud VPS (Ubuntu 24.04 LTS)

Welcome to this detailed guide on installing Magento 2 on a Google Cloud Virtual Private Server (VPS) running Ubuntu 24.04 LTS. Whether you're launching a new e-commerce platform or migrating an existing store, this repository provides a step-by-step walkthrough to help you get started.

IP: 34.89.22.193
Magento admin: http://34.89.22.193/admin_4j27d9y/

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

## Step 16: update permissions
```bash
sudo chown magento:www-data /var/www/html/
```

## Step 17: Generate keys
public key is login private key is password
```
https://commercemarketplace.adobe.com/customer/accessKeys/
```

## Step 18: Install composer
```bash
curl -sS https://getcomposer.org/installer | php
```

```bash
sudo mv composer.phar /usr/local/bin/composer
```


## Step 19: Switch User
```bash
sudo su magento
```

## Step 20: Change directory
```bash
cd /var/www/html
```

## Step 21: remove index.html
```bash
rm index.html
```

## Step 22: Install magento using composer
```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.7 .
```

## Step 23: Set pre-installation permissions
```bash
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} + && find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} + && chown -R :www-data . && chmod u+x bin/magento
```

## Step 24: Create user and db in phpmyadmin 

![Screenshot 2024-09-03 191156](https://github.com/user-attachments/assets/43d6b847-a4e9-4ccc-a485-4315eaead761)

## Step 25: Install elastric search

```bash
sudo apt install openjdk-11-jdk
```

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.14.0-amd64.deb
```

```bash
sudo dpkg -i elasticsearch-7.14.0-amd64.deb
```

```bash
sudo systemctl start elasticsearch
```

```bash
sudo systemctl enable elasticsearch
```

## Step 26: Setup magento using CLI

```bash
bin/magento setup:install \
--base-url="http://34.89.22.193/" \
--db-host="localhost" \
--db-name="magento-master" \
--db-user="magento-master" \
--db-password="password" \
--admin-firstname="Admin" \
--admin-lastname="User" \
--admin-email="admin@example.com" \
--admin-user="admin" \
--admin-password="password" \
--language="en_GB" \
--currency="GBP" \
--timezone="Europe/London" \
--use-rewrites="1"

```




## Step 27: if not working reinstall magento 

```bash
php bin/magento setup:uninstall
```

```bash
php bin/magento setup:install --base-url=http://34.89.22.193/ --db-host=localhost --db-name=magento-master --db-user=magento-master --db-password=Hy2karolxbt --admin-firstname=Admin --admin-lastname=User --admin-email=admin@example.com --admin-user=admin --admin-password=admin123 --language=en_US --currency=USD --timezone=America/Chicago --use-rewrites=1
```

## Step 28 Distable cache for developer mode :
```bash
php bin/magento cache:disable
php bin/magento cache:clean
php bin/magento cache:flush
```

## Step 29: Distable two factor authentication
```bash
php bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth
php bin/magento module:disable Magento_TwoFactorAuth
```

## Step 30: Update PHP memory limit in '/etc/php/8.3/apache2/php.ini' to 4GB
```bash
memory_limit = 4048M
```

## Step 31: Setup Cronjobs
```bash
php bin/magento cron:install
```

## Step 32: Enable Developer mode
```bash
php bin/magento deploy:mode:set developer
```

## Step 33: Clear the cached generated folders
```bash
rm -rf generated/metadata/* generated/code/*
```

## Step 34: Clear Cache
```bash
php bin/magento c:c
```

## Step 35: Enable Backups
Stores->Configuation

![image](https://github.com/user-attachments/assets/c7eade94-9891-4144-a9e6-11c746c51449)


Advanced->System

![image](https://github.com/user-attachments/assets/edfc8047-2269-4d64-a1ae-18fbe02440f1)

Backup Settings-> Change Enable Backup from No to Yes

![image](https://github.com/user-attachments/assets/a77b64dc-a5c5-4087-a77c-ccccceff69f0)

Click Save Config

![image](https://github.com/user-attachments/assets/13a8a5ff-97d4-4c1a-ab5b-a0ff1e70f359)

Refresh Configuration

![image](https://github.com/user-attachments/assets/17620403-6bf2-4b22-bc6a-fe280d051f24)

Go to System->Backups
![image](https://github.com/user-attachments/assets/e6303ef2-5d27-483a-9f48-030bfad5b042)

Click Database and Media Backup

![image](https://github.com/user-attachments/assets/ce2ed545-4d97-4724-8f3a-459495a712fb)

Set a name and click OK

![image](https://github.com/user-attachments/assets/3dc5b11a-1192-4b7d-875e-8935a049e439)

## Step 36: Upload Sample Data
```bash
php bin/magento sampledata:deploy
```

```bash
php bin/magento setup:upgrade
```



After installation I have this error 
![image](https://github.com/user-attachments/assets/4cb4d2fb-8a31-4779-a53d-1ca47965d6f6)

## Step 37: set permissions (user root user or sudo)
```bash
find /var/www/html -type f -exec chmod u+w {} \;
find /var/www/html -type d -exec chmod u+w {} \;
```








