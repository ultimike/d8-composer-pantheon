Enabling Twig Debugging on a Drupal 8 site
==========================================

Confirm your site is utilizing a `/sites/default/settings.local.php` file. If not, copy and rename 
`/sites/example.settings.local.php` to `/sites/default/settings.local.php`. Note that this file includes 
the `/sites/development.services.yml` file by default (around line 39). 

Edit your `/sites/development.services.yml` file and add the following to the "parameters" section:

`twig.config: { debug: true, auto_reload: true, cache: true, }`

The end result should look like this:

```
parameters:
  http.response.debug_cacheability_headers: true
  twig.config: { debug: true, auto_reload: true, cache: true, }
services:
  cache.backend.null:
    class: Drupal\Core\Cache\NullBackendFactory

Close and save the `/sites/development.services.yml` and rebuild caches.
