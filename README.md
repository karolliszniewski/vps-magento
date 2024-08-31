# Magento 2 Installation on Google Cloud VPS (Ubuntu 24.04 LTS)

Welcome to this detailed guide on installing Magento 2 on a Google Cloud Virtual Private Server (VPS) running Ubuntu 24.04 LTS. Whether you're launching a new e-commerce platform or migrating an existing store, this repository provides a step-by-step walkthrough to help you get started.

## Step 1: Update Ubuntu Repositories

Before we begin, ensure that your Ubuntu system is up to date by running the following command:

allow root user
```bash
sudo passwd root
```

```bash
sudo apt-get update
```

Install Apache2
```bash
sudo apt-get install apache2 -y
```

edit "/etc/apache2/sites-available/000-default.conf"

add
```
<Directory>
        AllowOverride All
</Directory>
```


