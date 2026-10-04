# Widget CSS Filter

This filter gives you access to the raw CSS generated from the widgets LESS stylesheets.

```php
/**
 * @param string $css The CSS.
 * @param array $instance The widget instance array.
 * @param SiteOrigin_Widget $widget The widget 
 */
function wbe_filter_widget_css( $css, $instance, $widget ){
    // Filter the CSS here.
    return $css;
}
add_filter('siteorigin_widgets_instance_css', 'wbe_filter_widget_css', 10, 3);
```

The filter runs only when the CSS is generated. The Widgets Bundle saves the CSS as a file in `wp-content/uploads/siteorigin-widgets/` and reuses that file while it exists. To see your changes while you work on the filter, add `define( 'SITEORIGIN_WIDGETS_DEBUG', true );` to your `wp-config.php` file. The LESS File and LESS Content filters work the same way.