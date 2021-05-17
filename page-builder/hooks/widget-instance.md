# Filtering Widget Instance

The `siteorigin_panels_widget_instance` hook will allow you to override the widget instance, and output content based on its values. It has three parameters:

- `instance`
  The current widget instance array
- `the_widget`
  The WP_Widget object of the current widget.
- `widget_class`
  A string containing the current widget_class name.

### Examples

The example code will enable the SiteOrigin Editor widget setting `Automatically Add Paragraph` if the `Title` setting is set to `Test`.

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
