# LESS File Filter

The `siteorigin_widgets_less_file_{$id_base}` filter changes the LESS file that styles a widget, where `{$id_base}` is the widget's base ID. [Extending Existing Widgets](../../getting-started/extending-existing-widgets.md) uses the filter to style a new button theme, and shows how the filter fits into extending a widget.

```php
function mytheme_button_less_file( $filename, $instance, $widget ) {
	if ( ! empty( $instance['design']['theme'] ) && $instance['design']['theme'] == 'test' ) {
		// This option works for plugins.
		$filename = plugin_dir_path( __FILE__ ) . 'less/test.less';

		// For a theme, use this line instead.
		// $filename = get_stylesheet_directory() . '/less/test.less';
	}
	return $filename;
}
add_filter( 'siteorigin_widgets_less_file_sow-button', 'mytheme_button_less_file', 10, 3 );
```
