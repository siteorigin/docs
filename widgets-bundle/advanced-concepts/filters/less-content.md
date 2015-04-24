# LESS content filter

This filter gives you raw access to the content of the LESS style file before the Widgets Bundle converts it into CSS. This filter runs after the LESS variables from the widget instance have been injected.

The filter takes the form `'siteorigin_widgets_less_' . $this->id_base`.

```php
/**
 * @param string $less The LESS content.
 * @param array $instance The widget instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_filter_widget_less( $less, $instance, $widget ) {
    // Filter the LESS content here.
    return $less;
}
add_filter('siteorigin_widgets_less_sow-button', 'wbe_filter_widget_less', 10, 3);
```