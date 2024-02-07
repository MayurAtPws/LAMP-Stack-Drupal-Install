# Installing LAMP Stack with Drupal

### System Information
- OS : Amazon Linux 2
- Storage : 10 GB
- Package manager : Yum

## LAMP Installation 

- Installing & Start **HTTPD** (Apache)
    
        sudo yum install httpd
        sudo systemctl start httpd
        sudo systemctl enable httpd.service

- Install & Start **MariaDB** (SQL)
  
        amazon-linux-extras enable mariadb10.5
        yum clean metadata
        yum install mariadb
        sudo systemctl start mariadb
        sudo systemctl enable mariadb.service

        # production safety
        sudo mysql_secure_installation

- Installing PHP 
        
        sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

        sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm

        sudo yum -y install yum-utils

        sudo yum-config-manager --disable 'remi-php*'

        sudo yum-config-manager --enable remi-php84

        sudo yum install php php-cli -y

        sudo yum install php php-mysql php-xml php-gd
        sudo systemctl restart httpd.service

- Checking PHP installation

        sudo chown -R ec2-user.ec2-user /var/www/html/
        nano /var/www/html/info.php

        # Add it to the file and save 
        <?php phpinfo(); ?>

- **check your site at :** http://your_server_IP_address/info.php


## Drupal Installation 

- Login & Create a New DB for our Drupal Project
 
        mysql -u root -p
        create database drupal_db;

- Create a New user & Set the Access rights

        CREATE USER db_user@localhost IDENTIFIED BY 'Durpal@123#';

        GRANT ALL PRIVILEGES ON drupal_db.* TO db_user@localhost;

        FLUSH PRIVILEGES;

        exit

- Download & Extract Drupal tar.gz
                
        curl -o drupal.tar.gz -L https://www.drupal.org/download-latest/tar.gz

        tar -zxvf drupal.tar.gz && mv drupal-* drupal

        #move to the web location 
        mv drupal /var/www/html/drupal

- Configuring settings 

        cd /var/www/html/drupal/sites/default/

        cp default.settings.php settings.php

        chown -R apache:apache /var/www/html/drupal/

        chcon -R -t httpd_sys_content_rw_t /var/www/html/drupal/

        chmod -R 777 /var/www/html/drupal/sites


- Your Drupal URL

        echo "http://$(curl -s ipaddress.sh)/drupal"

- Installing & Configuring Composer

        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

        php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

        php composer-setup.php

        php -r "unlink('composer-setup.php');"

        #adding to global

        mv composer.phar /usr/local/bin/composer/

        chmod +x /usr/local/bin/composer



