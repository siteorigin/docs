# Testing SiteOrigin plugins

There are a few automated PHPUnit tests and Selenium UI tests included with the plugins. Below we will lay out the steps necessary to run the tests.
  
## PHPUnit tests
We recommend the use of a pre-configured virtualized environment, like [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV) (VVV), as it provides a consistent environment for developing, testing, troubleshooting and debugging your code. VVV includes [WP-CLI](http://wp-cli.org/) and [PHPUnit](https://phpunit.de/) which make unit testing that much easier.

If you're using VVV you can skip step 1.
1) Install WP-CLI and PHPUnit
2) Initialize the unit tests using WP-CLI
  * In a terminal, navigate to the root of the WordPress instance and run `wp scaffold plugin-tests plugin-name` replacing `plugin-name` with the name of the plugin. So for the Widgets Bundle you would run `wp scaffold plugin-tests so-widgets-bundle`.
3) Navigate to the root of the plugin directory. `cd wp-content/plugins/so-widgets-bundle` and run `bash bin/install-wp-tests.sh wordpress_test db_user db_password localhost latest`, replacing `db_user` and `db_pass` with your database user's username and password and `localhost` with your database hostname.
4) That's it! Now you can run `phpunit` and you should see the output of the unit tests.

## Selenium UI tests

1) [Download](https://nodejs.org/download/) and install Node.js and npm.
2) In a terminal, navigate to the root of the plugin directory and run `npm install`
3) Get some coffee while npm installs the required packages.
4) That's it! Now you can run `npm test ./selenium-tests` and you should see a Chrome browser window open and the Selenium UI tests run.