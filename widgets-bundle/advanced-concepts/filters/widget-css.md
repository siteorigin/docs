# Widget CSS Filter

The `siteorigin_widgets_instance_css` filter changes the CSS that the Widgets Bundle compiles from a widget's LESS stylesheet.

The filter runs only when the Widgets Bundle compiles the CSS. The Widgets Bundle saves the CSS as a file in `wp-content/uploads/siteorigin-widgets/` and reuses the file while it exists. While you work on the filter, add `define( 'SITEORIGIN_WIDGETS_DEBUG', true );` to your `wp-config.php` file to see each change. The [LESS File Filter](less-file.md) and [LESS Content Filter](less-content.md) also run only when the Widgets Bundle compiles the CSS.

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
