# Widget Folders Filter

The `siteorigin_widgets_widget_folders` filter adds folders where the Widgets Bundle looks for widgets. The Widgets Bundle searches the folders for new widgets when a user opens **Plugins > SiteOrigin Widgets**, and loads active widgets from them.

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
