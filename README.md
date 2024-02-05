# Installing LAMP Stack with Drupal

### System Information
- OS : Amazon Linux 2
- Storage : 10 GB

## LAMP Installation 

- Installing & Start **HTTPD** (Apache)
    
        sudo yum install httpd
        sudo systemctl start httpd
        sudo systemctl enable httpd.service

- Install & Start **MariaDB** (SQL)
  
        sudo yum install mariadb-server
        sudo systemctl start mariadb
        sudo systemctl enable mariadb.service

        # production safety
        sudo mysql_secure_installation

- Installing PHP 
  
        sudo yum install php php-mysql
        sudo systemctl restart httpd.service

- Checking PHP installation

        sudo chown -R ec2-user.ec2-user /var/www/html/
        nano /var/www/html/info.php

        # Add it to the file and save 
        <?php phpinfo(); ?>

- **check your site at :** http://your_server_IP_address/info.php


## Drupal Installation 

