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

Add drush executable to your system PATH (~/.bash_profile):
```shell
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

## Use Drush to Download and Install Drupal

To install a new drupal site, we'll simply create a new directory then copy our make file inside and run [drush make](http://www.drush.org/en/master/make/) to download the codebase. The make file can pull any version drupal and any contrib modules we may need:

```shell
drush make drush.make.yml
```

Once we have the codebase we can set up the database using [sql-create](http://www.drushcommands.com/drush-7x/sql/sql-create):

```shell
drush sql-create --db-url=mysql://{user}:{pass}@localhost/{database}
```

and now we can run [site-install](http://www.drushcommands.com/drush-7x/site-install/site-install) to complete the installation:

```shell
drush site-install standard --account-name=admin --account-pass=admin --db-url=mysql://{user}:{pass}@localhost/{database}
```

(here's where we'd configure apache virtual hosts to point to our site ...)

We should now be able to access our freshly installed drupal site and login as 'admin'.

## Manage Multiple Sites with Aliases

Copy file 'aliases.drush.php' to ~/.drush directory

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





