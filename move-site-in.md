# General procedure to move a non-Composer Drupal 8 site into a Pantheon-focused, Composer-based site

## Scenario

*  Starting with a non-Composer, non-nested-docroot Drupal 8 site with an existing Git repository.  
*  The site is up-and-running in a local environment.  
*  _On my local, using "d8panelstest"._  

## Goals

*  Keep the existing repository.  
*  Composer-ize the code base.  
*  Utilize the pantheon-systems/example-drops-8-composer project.  
*  The existing local codebase will remain in-place and unchanged.  
*  The site will be available via a *new* Pantheon site.  

## Prerequisites: 
  *  Composer  
  *  Terminus  
  *  Git 
  
## Start from a local command-line in the directory where your updated project will go (usually ~/sites/):

`terminus site:create my_awesome_site "My Awesome Site" "Empty Upstream" --org='My Agency'`
 
`terminus connection:set my_awesome_site.dev git`

Optionally, enable Solr on the Pantheon site: `terminus solr:enable my_awesome_site`

Optionally, use HTTP authentication to lock the Pantheon Dev site: `terminus lock:enable my_awesome_site.dev username password`
 
`composer create-project pantheon-systems/example-drops-8-composer my_awesome_site`
 
`cd my_awesome_site`
 
`composer prepare-for-pantheon`
 
`composer install`
 
Remove "excludes" items from drupal-scaffold section of composer.json

Optionally, remove the entire "workflows" section of pantheon.yml
 
`composer drupal-scaffold`

Copy the `.git` directory from the original project to `/my-awesome-project/.git`. Be sure the local repository is up-to-date with any remote repositories prior to copying.

Copy any custom modules, themes, or profiles from the original project to the appropriate directory in `/my-awesome-project/web/`.

For contributed modules, themes, or profiles:

*  If you want to update to the latest versions, use `composer require drupal/project_name --prefer-dist` to install.  
*  If you want to keep the same versions as are currently installed on the original site, use `composer require drupal/project_name:version --prefer-dist` to install. After installation, you may want to manually edit the composer.json file and modify the version constraints for each module to allow for future updates.

Copy the files directory from original site to new site.  

Import a copy of the original project database to the new site database.

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

Clear all caches

Load site locally, confirm all is well. 

`git add .`

`git commit -m 'Moved site to a Composer-based workflow.'`

`terminus connection:info my_awesome_site.dev --field=git_url`

Use the results of the previous command in the next command.

Connect to the remote Pantheon Git repository:

*  If the site's repository is already hosted on Pantheon, then skip this step (but strongly consider doing all of this on a Pantheon Multidev environment).  
*  If the site's repository is hosted somewhere else, and you want to keep the ability to be able to push to both the original site repository and Pantheon, then use:
   *  `git remote add pantheon ssh://…` - you'll need to remember to substitute "pantheon" for "origin" in the remainder of Git commands below.  
*  If you want to only push this new repository to Pantheon then first remove the reference to the old repository:
   *  `git remote rm origin`  
   *  `git remote add origin ssh://…`  
*  If the site doesn't have an existing remote repository, the use:  
   *  `git remote add origin ssh://…`  

Push to the Pantheon repository:  

*  If the site's repository is already hosted on Pantheon use:  
   *  `git push origin master`  
*  If you are pushing to Pantheon for the first time use:  
   *  `git push --force origin master`  
   *  `git branch --set-upstream-to origin/master`  

Export the new local database. 

Import the new local database to Pantheon Dev. 

Copy the contents of the local files directory to Pantheon Dev.



#### Composer require command for Mike's d8panelstest site.
`composer require drupal/admin_toolbar --prefer-dist drupal/block_visibility_groups --prefer-dist drupal/bootstrap_paragraphs --prefer-dist drupal/contact_formatter --prefer-dist drupal/ctools --prefer-dist drupal/devel --prefer-dist drupal/entity_reference_revisions --prefer-dist drupal/page_manager --prefer-dist drupal/panels --prefer-dist drupal/paragraphs --prefer-dist drupal/viewsreference --prefer-dist drupal/bootstrap --prefer-dist`
