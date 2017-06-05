# Quick Reference: Creating a new Pantheon-focused, Drupal 8 site with Composer - LOCAL ENVIRONMENT ONLY

## Prerequisites: 
  *  Composer  
  *  Git  

## Start from a local command-line in the directory where your project will go (usually ~/sites/):
 
`composer create-project pantheon-systems/example-drops-8-composer my_awesome_site`
 
`cd my_awesome_site`
 
`composer prepare-for-pantheon`
 
`composer install`
 
Remove "excludes" items from drupal-scaffold section of composer.json
 
`composer drupal-scaffold`
 
`git init`
 
`git add .`
 
`git commit -m 'Initial commit.'`
 
Copy `/web/sites/example.settings.local.php` to `/web/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
```$config_directories['sync'] = '../config';
$settings['hash_salt'] = 'any_random_str';
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

Navigate to your local site and install Drupal.
