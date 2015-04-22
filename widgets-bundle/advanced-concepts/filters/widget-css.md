# Widget CSS filter

This filter gives you access to the raw CSS generated from the widgets LESS stylesheets.

```php
/**
 * @param string $css The CSS.
 * @param array instance The widget instance array.
 * @param SiteOrigin_Widget $widget The widget 
 */
function wbe_filter_widget_css( $css, $instance, $widget ){
}
add_filter('siteorigin_widgets_instance_css', 'wbe_filter_widget_css', 10, 3);