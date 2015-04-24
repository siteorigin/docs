# Active widgets filter

This filter changes which widgets are currently active. Most of the time you'll want to use `SiteOrigin_Widgets_Bundle::single()->activate_widget($id)` if all you want to do programmatically activate a widget. This filter is here if you have another use case though. You could, for example, force all the widgets you've created to be active, regardless of what the user sets in Plugins > SiteOriign Widgets.

```php
function wbexample_filter_active_widgets($active){
    $active['wbe-staff'] = true;
    return $active;
}
add_filter('siteorigin_widgets_active_widgets', 'wbexample_filter_active_widgets');
```

The array keys of `$active` correspond to the widget IDs. You can set them to either true or false.