# Widget Template HTML Filter

The `siteorigin_widgets_template_html_{$id_base}` filter changes the HTML that a widget's template outputs, where `{$id_base}` is the widget's base ID. Use it for small changes to the HTML. For larger changes, change the widget's template file with the [Template File Filter](template-file.md).

```php
/**
 * @param string $template_html The template HTML.
 * @param array $instance The widget instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_change_button_html( $template_html, $instance, $widget ) {
	// Modify the $template_html here

	return $template_html;
}
add_filter( 'siteorigin_widgets_template_html_sow-button', 'wbe_change_button_html', 10, 3 );
```
