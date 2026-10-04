# Filtering the Widget Instance

The `siteorigin_panels_widget_instance` filter changes a widget's instance right before Page Builder renders the widget on the front end, so you can change one setting based on the values of others. The filter passes three arguments:

- `$instance`: the widget's instance array.
- `$the_widget`: the widget's `WP_Widget` object.
- `$widget_class`: the widget's class name.

## Example

This example enables **Automatically add paragraphs** in the SiteOrigin Editor Widget when the widget's **Title** is "Test":

```
function so_editor_override_setting_if_test( $instance, $the_widget, $widget_class ) {
	if (
		$widget_class == 'SiteOrigin_Widget_Editor_Widget' &&
		! empty( $instance['title'] ) &&
		$instance['title' ] == 'Test'
	) {
		$instance['autop'] = true;
	}

	return $instance;
}
add_filter( 'siteorigin_panels_widget_instance', 'so_editor_override_setting_if_test', 10, 3 );
```
