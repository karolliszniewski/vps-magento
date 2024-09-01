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


