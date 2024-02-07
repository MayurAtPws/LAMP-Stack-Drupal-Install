# Updating drupal using composer

resource : https://www.drupal.org/docs/updating-drupal/updating-drupal-core-via-composer

- Update drupal/core in composer.json

        "require": {
            "drupal/core": "^10.2.2"
        }

- update db 

        drush updatedb

        drush updatedb
        (or)
        php core/update.php

        drush cr
        (or)
        php core/rebuild.php


