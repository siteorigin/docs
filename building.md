# Building SiteOrigin Plugins

There are few steps necessary to prepare a plugin for release on the WordPress.org plugin directory. We use [Gulp](http://gulpjs.com/) to automate this.

## Environment Setup

1. [Download](https://nodejs.org/download/) and install Node.js and npm.
2. In a terminal, navigate to the root of the plugin directory and run `npm install`
3. Get some coffee while npm installs the required packages.

## Running Builds

There are two build tasks, `build:release` and `build:dev`.

The release task performs the following subtasks:

1. Updates the version number in the `{plugin-name}.php` and `readme.txt` files.
2. Compiles LESS files to CSS.
3. Minifies JavaScript files and adds a `.min` suffix.
4. Copies all files to a `dist/{plugin-name}` folder.
5. Creates a `.zip` archive with the appropriate filename ready for uploading to wordpress.org.

Release task usage:

`gulp build:release -v version`

Where `version` should be replaced with the required version number.
For example, say the next version of the plugin is 1.2.3:

`gulp build:release -v 1.2.3`

The dev build task only has one subtask:

1. Watch LESS files for changes and compile to CSS.

This is simply to avoid having to manually recompile LESS files while working on them.

Test update.
