# Sanitize Instance Filter

The sanitize instance filters run last on a widget's instance before the Widgets Bundle saves it, so you can sanitize the instance further. `siteorigin_widgets_sanitize_instance` runs first, for every widget, and `siteorigin_widgets_sanitize_instance_{$id_base}` runs for one widget, where `{$id_base}` is the widget's base ID. Before either filter runs, the Widgets Bundle removes instance keys that don't belong to a form field.

```php
/**
 * @param array $instance The widget instance.
 * @param array $fields The form fields.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_sanitize_widget_instance($instance, $fields, $widget){
    // Use $fields to check and sanitize $instance
    return $instance;
}
add_filter('siteorigin_widgets_sanitize_instance_sow-button', 'wbe_sanitize_widget_instance', 10, 3);
```
