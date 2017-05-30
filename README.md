# Quick Reference: Creating a new Pantheon-focused, Drupal 8 site with Composer and Terminus

## Prerequisites: 
  *  Composer  
  *  Terminus  
  *  Git  

## Start from a local command-line in the directory where your project go (usually ~/sites/):
 
`terminus site:create my_awesome_site "My Awesome Site" "Empty Upstream" --org='My Agency'`
 
`terminus connection:set my_awesome_site.dev git`
 
`composer create-project pantheon-systems/example-drops-8-composer my_awesome_site`
 
`cd my_awesome_site`
 
`composer prepare-for-pantheon`
 
`composer install`
 
Remove "excludes" items from drupal-scaffold section of composer.json
 
`composer drupal-scaffold`
 
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
 
`terminus backup:create my_awesome_site.dev --element=database`
 
`terminus backup:get my_awesome_site.dev --element=database | xargs curl -o db.sql.gz`
 
Copy `/www/sites/example.settings.local.php` to `/www/sites/default/settings.local.php` and append (with the proper database credentials for your local) the following:
 
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

Uncompress and import `db.sql.gz` into your local environment.
