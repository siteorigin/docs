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
