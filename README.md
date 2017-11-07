# Quick Reference: Creating a new Pantheon-focused, Drupal 8 site with Composer and Terminus (Pantheon's Composer template)

## Prerequisites: 
  *  Composer  
  *  Terminus  
  *  Git  

## Start from a local command-line in the directory where your project will go (usually ~/sites/):
 
`terminus site:create my_awesome_site "My Awesome Site" "Empty Upstream" --org="My Agency"`
 
`terminus connection:set my_awesome_site.dev git`

Optionally, enable Solr on the Pantheon site: `terminus solr:enable my_awesome_site`

Optionally, use HTTP authentication to lock the Pantheondev site: `terminus lock:enable my_awesome_site.dev username password`
 
`composer create-project pantheon-systems/example-drops-8-composer my_awesome_site`
 
`cd my_awesome_site`
 
`composer prepare-for-pantheon`
 
`composer install`
 
Remove "excludes" items from drupal-scaffold section of composer.json

Optionally, remove the entire "workflows" section of pantheon.yml
 
`composer drupal-scaffold`

`mkdir database_dumps`
 
`git init`
 
`git add .`
 
`git commit -m 'Initial commit.'`

`terminus connection:info my_awesome_site.dev --field=git_url`
 
 Use the results of the previous command in the next command.
 
`git remote add origin ssh://… `
 
`git push --force origin master`
 
`git branch --set-upstream-to origin/master`
 
`terminus connection:set my_awesome_site.dev sftp`

`terminus remote:drush my_awesome_site.dev -- site-install --account-name=admin --account-pass=some-difficult-password --account-mail=you@domain.com`
 
`terminus connection:set my_awesome_site.dev git`

If using Kalabox for your local development stack, stop here and pull the site down using the Kalabox UI and follow the instructions at https://www.thinktandem.io/blog/2017/05/20/using-pantheon-s-nested-docroot-with-kalabox/ for using Kalabox with a nested docroot.

Else, if you're using another local development stack...
 
`terminus backup:create my_awesome_site.dev --element=database`
 
`terminus backup:get my_awesome_site.dev --element=database | xargs curl -o database_dumps/db.sql.gz`
 
Copy `/web/sites/example.settings.local.php` to `/web/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
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

Uncompress and import `db.sql.gz` into your local environment.
