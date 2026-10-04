# LESS Content Filter

You're able to access a widget's LESS prior to it being converted to CSS by using the `'siteorigin_widgets_less_' . $this->id_base` filter. This filter runs _after_ the LESS variables from the widget instance have been injected.

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

If you want to make changes to the LESS prior to the variables being replaced, you can use the `'siteorigin_widgets_less_vars_' . $this->id_base` filter instead.

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

Please note that `'siteorigin_widgets_less_vars_' . $this->id_base` is run after WB has run all LESS `.widget-function()` callbacks and processed all LESS @imports.

To change the value of a LESS variable, use the `'siteorigin_widgets_less_variables_' . $this->id_base` filter. It receives the variables array, the instance and the widget object. Unless the widget defines `get_style_hash_variables()`, the result is also part of the hash in the widget's CSS file name. Changes made through the other LESS filters don't change that hash, so an existing CSS file isn't regenerated.

```php
function wbe_filter_widget_less_variables( $vars, $instance, $widget ) {
	$vars['some_variable'] = '#ff0000';
	return $vars;
}
add_filter( 'siteorigin_widgets_less_variables_sow-button', 'wbe_filter_widget_less_variables', 10, 3 );
```
