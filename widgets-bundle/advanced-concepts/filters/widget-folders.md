# Widget folders filter

This filter gives you a way to add extra folders to the Widgets Bundle. It searches these folders for new widgets when the user goes to Plugins > SiteOrigin Widgets and checks these folders when widgets are active.

```php
function wbexample_add_widget_folders( $folders ){
    // Add a theme folder
    $folders[] = get_template_directory() . '/widgets/';
    // Or add a plugin folder
    $folders[] = plugin_dir_path(__FILE__) . 'widgets/';
    
    return $folders;
}
add_filter('siteorigin_widgets_widget_folders', 'wbexample_add_widget_folders');
```