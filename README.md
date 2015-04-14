# DCWP - Wordpress Project Base

Base WordPress starter project built on [Roots/Bedrock](https://roots.io/bedrock/).

# ToC

* [Setup](#setup)
	* [Setup New Project](#setup-new-project)
	* [Configure Local Project](#configure-local-project)
	* [Questions](#questions)
* [Additional Documentation](#additional-documentation)
	* [Configuration Files](#configuration-files)
	* [Environment Variables](#environment-variables)
	* [Composer](#composer)
	* [Plugins](#plugins)
	* [Capistrano](#capistrano)
* [Updates](#updates)


## Setup 

### Setup New Project

1. Clone project repo
  * Go to your Sites folder: `cd ~/Sites`.
  * Run `git clone https://github.com/Design-Collective/dcwp myproject`
2. Create remote repository
  * Go to [https://github.com/new](https://github.com/new) and setup the new repo
  * follow the instructions to update the remote url, e.g.:
  `git remote set-url origin https://github.com/Design-Collective/my-new-repo`
3. Run `composer install`
4. If you wish to clone the [DesignCollective/sage](https://github.com/Design-Collective/sage) starter theme, follow the instructions in the repository.
5. Push your local repo to your new repository: `git push origin master`

### Configure local project
6. Copy `.env.example` to `.env` and update environment variables:
  * `DB_NAME` - Database name
  * `DB_USER` - Database user
  * `DB_PASSWORD` - Database password
  * `DB_HOST` - Database host (defaults to `localhost`)
  * `WP_ENV` - Set to environment (`development`, `staging`, `production`, etc)
  * `WP_HOME` - Full URL to WordPress home (http://example.com)
  * `WP_SITEURL` - Full URL to WordPress including subdirectory (http://example.com/wp)
7. Access WP Admin at `http://project.dev/wp/wp-admin`


### Questions?
For more information, see [Bedrock](https://roots.io/bedrock/)


## Additional Documentation

### Configuration Files

To manually add an environment variable, add a line to the `.env` file:
```
MYVAR="My variable value"
MYCUSTOMNUMBER=999
```

`config/application.php` is the main config file that contains what `wp-config.php` usually would. Base options should be set in there.

For environment specific configuration, use the files under `config/environments`. By default there's is `development`, `staging`, and `production` but these can be whatever you require.

The environment configs are required **before** the main `application` config so anything in an environment config takes precedence over `application`.

Note: You can't re-define constants in PHP. So if you have a base setting in `application.php` and want to override it in `production.php` for example, you have a few options:

* Remove the base option and be sure to define it in every environment it's needed
* Only define the constant in `application.php` if it isn't already defined.

### Environment Variables

The following env vars are required:

* `DB_USER`
* `DB_NAME`
* `DB_PASSWORD`
* `WP_HOME`
* `WP_SITEURL`

/* 

Note: Need to review this and add all useful vars

Additional variables which can be included in the global theme:
* `GOOGLE_ANALYTICS_PUBID`
* `INSTAGRAM_USER_ID`

*/

### Composer

[Composer](http://getcomposer.org) is used to manage dependencies. Bedrock considers any 3rd party library as a dependency including WordPress itself and any plugins.

See these two blogs for more extensive documentation:

* [Using Composer with WordPress](https://roots.io/using-composer-with-wordpress/)
* [WordPress Plugins with Composer](https://roots.io/wordpress-plugins-with-composer/)

#### Plugins

[WordPress Packagist](http://wpackagist.org/) is already registered in the `composer.json` file so any plugins from the [WordPress Plugin Directory](http://wordpress.org/plugins/) can easily be required.

Search for a plugin on [WordPress Packagist](http://wpackagist.org/)

To add a plugin, add it under the `require` directive or use `composer require <namespace>/<packagename>` from the command line. If it's from WordPress Packagist then the namespace is always `wpackagist-plugin`.

For example: `"wpackagist-plugin/my-plugin": "dev-trunk"`

Whenever you add a new plugin or update the WP version, run `composer update` to install your new packages.

`plugins`, and `mu-plugins` are Git ignored by default since Composer manages them. If you want to add something to those folders that *isn not* managed by Composer, you need to update `.gitignore` to whitelist them, for example:

`!web/app/plugins/my-plugin-name`

Note: Some plugins may create files or folders outside of their given scope, or even make modifications to `wp-config.php` and other files in the `app` directory. These files should be added to your `.gitignore` file as they are managed by the plugins themselves, which are managed via Composer. Any modifications to `wp-config.php` that are needed should be moved into `config/application.php`.


### Capistrano

[Capistrano](http://www.capistranorb.com/) is a remote server automation and deployment tool. It will let you deploy or rollback your application in one command:

* Deploy: `cap staging deploy`
* Rollback: `cap staging deploy:rollback`

Composer support is built-in so when you run a deploy, `composer install` is automatically run on the server.


## Updates
If you plan to merge/update this repo from [Roots/Bedrock](https://roots.io/bedrock/) you will need to setup the `upstream` remote in order to merge/update from upstream.

Run the following commands:
  * `git remote add upstream https://github.com/roots/bedrock`
  * `git fetch upstream`
  Commits to the remote master will be stored in a local branch, upstream/master.
  * Checkout local master branch: `git checkout master`
  * Merge upstream/master branch to sync with upstream: `git merge upstream/master`