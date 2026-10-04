# Getting Started Quickly

The fastest way to build a widget is to copy our Hello World Widget. Install and activate the [SiteOrigin Widgets Bundle](https://wordpress.org/plugins/so-widgets-bundle/), clone our [so-dev-examples](https://github.com/siteorigin/so-dev-examples) Git repository, and activate its `extend-widgets-bundle` plugin at **Plugins**. The plugin's `hello-world-widget` folder holds the Hello World Widget, and these steps turn a copy into your own widget:

1. Choose an ID for your widget, such as `my-awesome-widget`.
2. Copy the `hello-world-widget` folder, and rename the copy and the `hello-world-widget.php` file inside it with your ID, such as `my-awesome-widget` and `my-awesome-widget.php`. The folder and the file must have the same name.
3. Open the PHP file and rename the `Hello_World_Widget` class, for example to `My_Awesome_Widget`.
4. In the class constructor, the first argument to the parent constructor is the widget ID. Replace `hello-world-widget` with your ID.
5. At the bottom of the file, the widget is registered. Replace `hello-world-widget` with your ID and `Hello_World_Widget` with your class name.
6. In the metadata header above the class, change the `Widget Name` field to your widget's name, such as `Widget Name: My Awesome Widget`. The name can be anything, but the Widgets Bundle lists the widget only if the header has a `Widget Name` field.
7. Go to **Plugins > SiteOrigin Widgets** and activate your widget. New widgets start inactive.

You now have a working widget to build on.

## Adding a Separate Widgets Folder

The `siteorigin_widgets_widget_folders` filter registers a folder of your own widgets, so you can keep them apart from the SiteOrigin widgets:

```php
<?php

function add_my_awesome_widgets_collection($folders){
    $folders[] = plugin_dir_path( __FILE__ ) . 'extra-widgets/';
    return $folders;
}
add_filter('siteorigin_widgets_widget_folders', 'add_my_awesome_widgets_collection');
```

The Widgets Bundle looks for PHP files in each subfolder of a registered folder. It lists every file whose metadata header has a `Widget Name` field as a widget that users can activate and use wherever widgets work.

The `extend-widgets-bundle` example plugin is a standard WordPress plugin that uses this filter to add its `extra-widgets` folder.
