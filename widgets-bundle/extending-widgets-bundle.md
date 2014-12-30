# Extending the widgets bundle

## Getting started quickly

The quickest and simplest way to get started is by cloning our [so-dev-examples](../../../so-dev-examples) git repository. Look in the `extend-widgets-bundle` plugin (remember to activate it under Plugins) and using the Hello World Widget as a template. All you need to do is create a copy of the `hello-world-widget` folder and rename it as follows:

1. Choose an id for your widget. E.g. `my-awesome-widget`.
2. Copy the `hello-world-widget` folder and rename it, and the `hello-world-widget.php` file inside, using your chosen id. E.g. the folder will be `my-awesome-widget` and the file will be `my-awesome-widget.php`. It is important that these are the same.
3. Open the PHP file and rename the class from `Hello_World_Widget` to a name of your choice. E.g. `My_Awesome_Widget`
4. In the class' constructor, the first argument to the parent constructor is the widget id. Replace the current value `hello-world-widget` with your chosen id. E.g. `my-awesome-widget`.
5. At the bottom of the file, you'll see the widget being registered. Again replace `hello-world-widget` with your chosen id and replace the class name `Hello_World_Widget`, with your class name. E.g. `my-awesome-widget` and `My_Awesome_Widget`.
6. In the metadata header above the class, change the `Widget Name` field to your widget name. E.g. `Widget Name: My Awesome Widget`. This field does not follow any convention, but it must be present for your widget to be included in the list of SiteOrigin widgets.

You should now have a simple functional widget that you can start changing to create your awesome widget!

## Adding a separate widgets folder

If you'd like to keep your widgets separate from the SiteOrigin widgets, we have included a filter hook which you can use to register a folder containing several of your widgets, as follows:

```php
<?php

function add_my_awesome_widgets_collection($folders){
	$folders[] = 'path/to/my/widgets/';
	return $folders;
}
add_filter('siteorigin_widgets_widget_folders', 'add_my_awesome_widgets_collection');
```

The Widget Bundle plugin code will check subfolders of this folder for PHP files. If it finds any PHP files with a metadata header containing a Widget Name field, it will list them as a widget which can be activated and used anywhere widgets may normally be used.

In our example `extend-widgets-bundle` plugin we use the standard WordPress method of creating a plugin, which then uses the above filter hook to add it's `extra-widgets` folder to the search path for widgets.

## Getting started slowly

What follows are the basic requirements for creating your own widget using the SiteOrigin Widgets Bundle as a framework.

#### Widget name

Start by creating your wiget folder using a name of your choice, and then a php file with the same name. We encourage the use of the WordPress guidelines for naming files and folders, which you can find [here](http://codex.wordpress.org/Writing_a_Plugin#Names.2C_Files.2C_and_Locations).

#### Widget metadata

The first thing you'll need in your PHP file is the metadata header which is used by the SiteOrigin Widgets Bundle plugin to identify PHP files which contain a widget class. The minimum requirement for this header is the Widget Name field, as without this field the file will be skipped. The rest of the fields are optional but we encourage their use, as they provide the option to display more detailed information about the widget and/or widget author in future.

```php
<?php

/*
Widget Name: Hello world widget
Description: An example widget which displays 'Hello world!'.
Author: Me
Author URI: http://example.com
Widget URI: http://example.com/hello-world-widget-docs,
Video URI: http://example.com/hello-world-widget-video
*/

```

#### Widget class

Now you'll need to create a class which extends the `SiteOrigin_Widget` abstract base class and, as a minimum, overrides the `get_template_name` and `get_style_name` abstract methods. You'll also need to register your widget class with the SiteOrigin Widgets Bundle using the `siteorigin_widget_register` function, passing in the widget id, widget file path, and widget class name as arguments.

```php
<?php

class Hello_World_Widget extends SiteOrigin_Widget {

	function get_template_name($instance) {
		return '';
	}

	function get_style_name($instance) {
		return '';
	}
}

siteorigin_widget_register('hello-world-widget', __FILE__, 'Hello_World_Widget');
```

Once you've done this, you'll see your widget in the Plugins > SiteOrigin Widgets list, it can be activated and deactivated, and you'll see an 'Untitled Widget' in Page Builder widgets and a blank widget in other widget lists. So you can't really use your widget yet, but it's there!
