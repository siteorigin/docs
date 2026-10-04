# Building SiteOrigin Plugins

We use [Gulp](https://gulpjs.com/) to prepare our plugins for release on the WordPress.org plugin directory. The build tasks live in the [plugin-build](https://github.com/siteorigin/plugin-build) repository, which each plugin includes as a Git submodule in a folder called `build`.

## Environment Setup

1. [Download](https://nodejs.org/download/) and install Node.js and npm. The build uses `node-sass` 4.14, which supports Node.js 14 and earlier.
2. In a terminal, navigate to the root of the plugin directory and run `git submodule update --init build` to add the `build` folder.
3. Navigate to the `build` folder and run `npm install`. For SiteOrigin CSS, also run `npm install` in the plugin root.

Each plugin has a `build-config.js` file in its root folder, which tells the build the plugin slug and which files to version, compile and copy.

## Running Builds

The build has two tasks, `build:release` and `build:dev`, and you run both from the `build` folder. The release task:

1. Updates the version number in the files listed under `version.src` in `build-config.js`.
2. Compiles LESS and SASS files to CSS.
3. Processes JavaScript files with Babel and Browserify, if `build-config.js` sets them up.
4. Minifies JavaScript files with the suffix set in `jsMinSuffix` in `build-config.js`, and minifies the CSS files listed under `css.src` with a `.min` suffix.
5. Generates the translation (POT) file.
6. Copies all files to a `dist/{slug}` folder.
7. Creates a `{slug}.{version}.zip` archive ready to upload to WordPress.org.

Run the release task with `npx`, which uses the Gulp version installed in the `build` folder, and replace `1.2.3` with the release number:

`npx gulp build:release -v 1.2.3`

The Widgets Bundle uses a newer version of the build, which needs Node.js 18 or later and doesn't use `node-sass`. Its Gulp command line reads `-v` as a request for the Gulp version, so run the release task through npm instead:

`npm run build:release --release=1.2.3`

The dev task compiles the LESS and SASS files, and any Babel and Browserify sources set up in `build-config.js`. It then watches those files and recompiles them when you save a change. Run it with `npx gulp build:dev`, or `npm run build:dev` in the Widgets Bundle.
