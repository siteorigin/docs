#Widget template HTML filter

This filter gives you some very fine grained control over the HTML displayed for a widget. The filter has the form `'siteorigin_widgets_template_html_' . $this->id_base`. It might be better to just change the template file being used for a given widget, but this filter is ideal if you just need to make small changes.

```php
/**
 * @param string $template_html The template HTML.
 * @param array $instance The widget instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_change_button_html( $template_html, $instance, $widget ){
	// Modify the $template_html here
	
	return $template_html
}
add_filter('siteorigin_widgets_template_html_sow-button', 'wbe_change_button_html', 10, 3);
```
