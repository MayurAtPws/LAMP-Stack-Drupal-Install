# Installing LAMP Stack with Drupal

### System Information
- OS : Amazon Linux 2023
- Storage : 10 GB
- Package manager : DNF
- PHP : v8.2
- MariaDB : v10.5

## LAMP Installation 
resource : https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html
- Installing & Start LAMP 
    
        sudo dnf update -y

        sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel php-xml php-gd

        sudo systemctl start httpd
        sudo systemctl enable httpd.service

        sudo dnf install mariadb105-server
        sudo systemctl start mariadb
        sudo systemctl enable mariadb.service

## DB setup for Drupal 

        # production safety
        sudo mysql_secure_installation
- Login & Create a New DB for our Drupal Project
 
        mysql -u root -p
        create database drupal_db;

- Create a New user & Set the Access rights

        CREATE USER db_user@localhost IDENTIFIED BY 'Durpal@123#';

        GRANT ALL PRIVILEGES ON drupal_db.* TO db_user@localhost;

        FLUSH PRIVILEGES;

        exit
 
# Install Composer (Global)

resource : https://getcomposer.org/download/

- Execute this to install composer

        echo '#!/bin/sh

        EXPECTED_CHECKSUM="$(php -r '\''copy("https://composer.github.io/installer.sig", "php://stdout");'\'')"
        php -r "copy('"'"'https://getcomposer.org/installer'"'"', '"'"'composer-setup.php'"'"');"
        ACTUAL_CHECKSUM="$(php -r "echo hash_file('"'"'sha384'"'"', '"'"'composer-setup.php'"'"');")"

        if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]
        then
            >&2 echo '"'"'ERROR: Invalid installer checksum'"'"'
            rm composer-setup.php
            exit 1
        fi

        php composer-setup.php --2.2 --install-dir=/usr/local/bin --filename=composer --quiet
        RESULT=$?
        rm composer-setup.php
        exit $RESULT' > compose.sh



        #### Install It ####

        chmod +x compose.sh

        ./compose.sh


- Check the Installation :

        composer --version

# Install Drush (Global)

resource : https://drupalize.me/tutorial/install-drush-using-composer

- Download the drush & Add to path

        wget -O drush.phar https://github.com/drush-ops/drush-launcher/releases/latest/download/drush.phar

        chmod +x drush.phar

        sudo mv drush.phar /usr/local/bin/drush

# Install Drupal using composer 

resource : 

1 - https://www.drupal.org/docs/develop/using-composer/manage-dependencies

2 - https://www.atlantic.net/dedicated-server-hosting/how-to-install-drupal-on-fedora/

- download drupal 

        composer create-project drupal/recommended-project drupal

- Move to the Web server root folder 

        mv drupal /var/www/html/drupal

- Configuring settings 

        cd /var/www/html/drupal/web/sites/default/

        cp default.settings.php settings.php

        chown -R apache:apache /var/www/html/drupal/

        chcon -R -t httpd_sys_content_rw_t /var/www/html/drupal/

        chmod -R 777 /var/www/html/drupal/web/sites


# Configuring HTTPD Server 

fix resource : https://stackoverflow.com/questions/18853066/404-not-found-the-requested-url-was-not-found-on-this-server

- tweak the conf

        nano /etc/httpd/conf/httpd.conf

        #change the root 
        DocumentRoot /var/www/html/drupal/web/

        (or)

        #change /drupal as the folder
        Alias /drupal /var/www/html/drupal/web/

- Fix for clean URLS on the conf

        <Directory var/www/html/drupal>
            Options FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

-  Restart it 

        sudo systemctl restart httpd


- Your Drupal URL

        echo "http://$(curl -s ipaddress.sh)/drupal"


###  Complete the installation using the GUI Setup. Refer  [credentials.txt](./credentials.txt) for sample data

## Error Fix:
Fix Drupal Links and GUI : [Click here](update.md)


## Updating Drupal using composer 
Updating Drupal : [Click here](./update.md)
