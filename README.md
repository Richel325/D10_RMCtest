# About This Repo

todo: Add some explanation of the repo.

## Development workflow

These are the basic steps for this project. We'll add more and make modifications as we go.

### Reset from main and create a new branch

For each new feature, the developer should create a feature branch from `main` that represents an incremental change to the code and configuration for this project. For now, keep branch names simple and concise, two to three words. For example `enable-facets` or `install-pathauto`, etc. (We may add in the inclusion of an issue ID or issue type later.)

As part of creating a feature branch, it is a good thing to include commands to ensure you have any upstream changes. 

```
git checkout main
git pull
ddev composer install
ddev drush deploy
git checkout -b [branch name]
```

You could also use the following aliases if using the [OhMyZsh git plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/git/git.plugin.zsh).

```
gco main
gl
ddev composer install
ddev drush deploy
gcb [branch name]
```

In the above, `composer install` after pulling the main branch insures you have the latest module/package dependencies.

`drush deploy` is basically shorthand for:

```
drush updatedb --no-cache-clear
drush cache:rebuild
drush config:import
drush cache:rebuild
drush deploy:hook
```

### Exporting your config changes

After you are on a clean feature branch, you can proceed with making edits to your site's configuration and add custom code and styles to your site. When you are done making configuration changes, or if you just want to save progress, you can export your config.

```
drush config:export
git add .
git commit -m "Concise description of your change."
```

Or with aliases:

```
drush cex -y
gaa
gcmsg "Concise description of your change."
```

### Creating a pull request

When you are ready to push your changes upstream and create a pull request against main:

```
git push --set-upstream origin [current branch]
```

This command will provide you with a link for creating a pull request on Github, but you may also just visit the repo to see there is a change that can be turned into a pull request.

You can create additional commits after creating a pull request that if pushed are added to that request.

### Merge conflicts and rebasing

If you see a merge conflict after creating the pull request, you may need to rebase your branch with other changes that have been committed to `main` that conflict with your branch.

To rebase:

```
gco main
git pull
gco [feature branch]
git rebase main
[resolve errors using merge tool]
gaa  //if needed
git rebase --continue //if needed
ddev composer install
ddev drush deploy
```
Those last two commands are extremely important. If you've pulled in work completed by someone else, it may have dependencies (composer) or configuration that needs to be added to your code. If you don't run these commands, you may undo those changes with your next config export.

After a successful rebase, you'll need to force push your commit to reflect the new history.

```
git push --force
```

Or the alias

```
gpf!
```

A force push overwrites the branch on your upstream remote, so use it with care. Checking `git log` or using the version control feature of your IDE helps you understand that the changes you are submitting are the ones you expect.
