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

Optionally, remove the entire "workflows" section of pantheon.yml
 
`composer drupal-scaffold`
 
`git init`
 
`git add .`
 
`git commit -m 'Initial commit.'`
 
Copy `/web/sites/example.settings.local.php` to `/web/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
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


// Files directories - uncomment to override
//$settings['file_public_path'] = 'sites/default/files';
//$settings['file_private_path'] = 'sites/default/files/private';
//$config['system.file']['path']['temporary'] = '/tmp';

// Stage File Proxy
/*
$config['stage_file_proxy.settings']['origin'] = 'http://example.com'; // no trailing slash
$config['stage_file_proxy.settings']['origin_dir'] = 'sites/default/files';
*/

// Environment Indicator
/*
$config['environment_indicator.indicator']['bg_color'] = 'green';  // green for local, yellow for Dev, red for Live.
$config['environment_indicator.indicator']['fg_color'] = 'white';
$config['environment_indicator.indicator']['name'] = 'Local';
*/
```

Navigate to your local site and install Drupal.
