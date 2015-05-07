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

To install a new drupal site, we'll simply create a new directory, grab our make file and run [drush make](http://www.drush.org/en/master/make/) to download the codebase. The make file can pull any version drupal along with any contrib modules we may need:

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

Using site [aliases](http://drush.readthedocs.org/en/master/shellaliases/) we can easily run drush on any drupal installation running on any server.

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

See a list of all available drush site aliases:

```shell
drush sa
```

## Syncing Live and Dev Sites

Now that we have aliases set up, we can sync content and configuration changes between multiple instances.

For example, we can run [sql-sync](http://www.drushcommands.com/drush-6x/sql/sql-sync) to clone the live database to dev:

```shell
drush sql-sync @live @dev
```

Similarly we can copy any file uploads using the [rsync](http://www.drushcommands.com/drush-7x/core/core-rsync) command:

```shell
drush rsync @live:%files @dev:%files
```

## Everyday Commands

Clear all the caches!

```shell
# drupal 7
drush cc all

# drupal 8
drush cache-rebuild
```

Manage contrib modules:

```shell
# download
drush dl {module}

#enable
drush en {module}

#disable
drush dis {module}
```

Update core and all contrib modules:

```shell
drush pm-update
```

Login as another user

```shell
drush user-login {username}
```

Reset admin password

```shell
drush upwd admin --password="password"
```

## Even More Commands

List all enabled contrib modules:

```shell
drush pm-list --pipe --type=module --status=enabled --no-core
```

Run cron:

```shell
drush core-cron
```

Flush imagecache:

```shell
drush image-flush {image-style-id}
```	

View documentation:

```shell
drush topic
```
