# LESS Content Filter

The `siteorigin_widgets_less_{$id_base}` filter changes a widget's LESS before the Widgets Bundle compiles it to CSS, where `{$id_base}` is the widget's base ID. The filter runs after the Widgets Bundle adds the values of the widget's LESS variables.

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

The `siteorigin_widgets_less_vars_{$id_base}` filter changes the LESS before the Widgets Bundle adds the variables:

```php
/**
 * @param string $less The LESS content.
 * @param array $vars The widget LESS variables.
 * @param array $instance The widget instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_filter_widget_less_vars( $less, $vars, $instance, $widget ) {
	// Filter the LESS content here.
	return $less;
}
add_filter( 'siteorigin_widgets_less_vars_sow-button', 'wbe_filter_widget_less_vars', 10, 4 );
```

`siteorigin_widgets_less_vars_{$id_base}` runs after the Widgets Bundle has run every `.widget-function()` callback and processed every `@import`.

The `siteorigin_widgets_less_variables_{$id_base}` filter changes the value of a LESS variable. It receives the variables array, the instance and the widget object. Unless the widget defines `get_style_hash_variables()`, the filter's result is also part of the hash in the widget's CSS file name. Changes from the other LESS filters don't change that hash, so they don't regenerate an existing CSS file.

```php
function wbe_filter_widget_less_variables( $vars, $instance, $widget ) {
	$vars['button_color'] = '#ff0000';
	return $vars;
}
add_filter( 'siteorigin_widgets_less_variables_sow-button', 'wbe_filter_widget_less_variables', 10, 3 );
```
