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

Create a new file inside the new `.docksal` directory named `docksal-local.env` with the following contents:

`DOCROOT=web`

Start up the Docksal containers for this project with: `fin start`

Copy `/projectname/web/sites/example.settings.local.php` to `/projectname/web/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
```$config_directories['sync'] = '../config';
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

Consider modifying the `hash_salt` variable to your own random string. Note that these database credentials are the default credentials for a standard Docksal configuration.

At this point, you should be able to go to http://projectname.docsal in your brower and be redirected to the Drupal install page. Do the installation (you should *not* be prompted for database credentials). 

