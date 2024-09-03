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

## Step 25: Setup magento using CLI

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





