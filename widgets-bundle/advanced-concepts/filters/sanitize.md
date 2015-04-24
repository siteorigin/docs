# Sanitize instance filter

The sanitization filter is the last filter run on the widget instance before we store it in the database. The Widgets Bundle does a lot of filtering, but you might want to add some custom sanitization.

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