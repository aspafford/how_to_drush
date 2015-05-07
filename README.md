# How to Drush

> A guide to using Drush command line tools.

## "What is Drush?"

[Drush](http://www.drush.org) is a command line shell and Unix scripting interface for Drupal.

## Install with Composer

[Get composer](https://getcomposer.org/doc/00-intro.md#globally):
```shell
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

[Install Drush](http://docs.drush.org/en/master/install/):
```shell
composer global require drush/drush:dev-master
```

Add 'drush' to your system PATH (~/.bash_profile):
```shell
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

## Use Drush to Download and Install Drupal

```shell
drush make drush.make.yml
```

and now let's install the site with drush

[first create the database ...]

```shell
drush site-install standard --account-name=admin --account-pass=admin --db-url=mysql://{user}:{pass}@localhost/{database}
```

[... and here's where we'd configure dns, restart apache, etc.]

We should now be able to access our freshly installed drupal site and login as 'admin'.

## Manage Multiple Sites with Aliases

Create file 'aliases.drush.php' at ~/.drush

```php
$aliases['dev'] = array(
  'root' => '/var/www/html/drush-dev',
  'uri' => 'drush-dev.local',
);
```

Now we can target our dev instance from anywhere on the host machine:

```shell
drush @dev status
```

See a list of all available drush aliases:

```shell
drush sa
```

## Syncing Live and Dev Sites

Now that we have aliases set up, we can sync data between multiple instances. For example, we can grab content from the live site and update our dev instance like this:

```shell
drush sql-sync @live @dev
```





