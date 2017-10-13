# Quick Reference: Creating a new local Composer-based Drupal 8 site (based on https://github.com/drupal-composer/drupal-project) using Docksal.

## Prerequisites: 
  *  Composer  
  *  Git  
  *  Docksal  

## Start from a local command-line in the directory where your project will go (usually ~/sites/):

Be sure to replace "some-dir" with the desired machine name ("projectname") for your project (all lowercase, no spaces).
 
`composer create-project drupal-composer/drupal-project:8.x-dev projectname --stability dev --no-interaction`

`cd projectname`

`mkdir .docksal`

Create a new `/projectname/.docksal/docksal-local.env` file with the following contents:

`DOCROOT=web`

Start up the Docksal containers for this project with: `fin start`

Copy `/projectname/web/sites/example.settings.local.php` to `/projectname/web/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
```
$config_directories['sync'] = '../config';
$settings['hash_salt'] = 'any_random_str';
$settings['file_chmod_directory'] = 0777;
$settings['file_chmod_file'] = 0666;
$databases['default']['default'] = array (
  'database' => 'default',
  'username' => 'user',
  'password' => 'user',
  'prefix' => '',
  'host' => 'db',
  'port' => '3306',
  'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
  'driver' => 'mysql',
);
```

Consider modifying the `hash_salt` variable to your own random string. Note that these database credentials are the default credentials for a standard Docksal configuration.

Next, modify `/projectname/web/sites/default/settings.php` and uncomment the 3 lines near the bottom of the file related to the settings.local.php:

```
if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {
  include $app_root . '/' . $site_path . '/settings.local.php';
}
```

At this point, you should be able to go to http://projectname.docsal in your brower and be redirected to the Drupal install page. Do the installation (you should *not* be prompted for database credentials). 

If you want to add/modify services (containers) to your project, create a `/projectname/.docksal/docksal-local.yml` to define the services you would like to run. Here's an example file:

```
version: "2.1"

services:
  # Adds a Solr container with version 3.x of Solr - see
  #  http://docksal.readthedocs.io/en/master/tools/apache-solr/ and 
  #  https://hub.docker.com/r/docksal/solr/tags/.
  # The "ports" and "environment" configuration allows the Solr server to be interacted
  #  directly with at http://192.168.64.100:8983/solr/
  #solr:
  #  hostname: solr
  #  image: docksal/solr:1.0-solr3
  #  ports:
  #   - "8983:8983"
  #  environment:
  #    - DOMAIN_NAME=solr.${VIRTUAL_HOST}
  # Changes the command-line interface to use the -dev version of the PHP 7.x container - 
  #  see http://docksal.readthedocs.io/en/master/advanced/stack-settings/ and 
  #  https://hub.docker.com/r/docksal/cli/tags/.  
  cli:
    image: docksal/cli:edge-php7
    #image: docksal/cli:1.3-php5
    #image: docksal/cli:1.3-php7
    # Uncomment to add support for xdebug - see http://docksal.readthedocs.io/en/master/tools/xdebug/.  
#    environment:
#      - XDEBUG_ENABLED=1
```
