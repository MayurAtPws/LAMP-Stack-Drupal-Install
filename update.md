# Updating drupal using composer


resource : https://www.drupal.org/docs/updating-drupal/updating-drupal-core-via-composer

drush install dev : https://drupalize.me/tutorial/install-drush-using-composer

⚠️ Note : Recommended to use drush as dev dependency (check the above link)

- Update drupal/core in composer.json

        "require": {
            "drupal/core": "^10.2.2"
        }

- update using composer 
  
        composer update "drupal/core-*" --with-all-dependencies

- making sure to Add Drush with Composer

        cd "/var/www/html/drupal" && composer require drush/drush


- update db 

        drush updatedb

        drush updatedb
        (or)
        php core/update.php

        drush cr
        (or)
        php core/rebuild.php


