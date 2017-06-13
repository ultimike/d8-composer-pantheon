## Keeping a feature branch up-to-date with commits from the "master" branch.

### Assume we're starting on a feature branch named "sword", in the `/web/` directory.

Get the sword branch configuration up-to-date.

```
drush config-export
git add ../config
git commit -m 'Updated config from sword branch.'
```

Switch to master branch and pull in any changes.

```
git checkout master
git pull --rebase
drush config-import
```

Go back to the sword branch and merge in changes (assume no code conflicts) from the master branch.

```
git checkout sword
git merge master
drush config-import
```
