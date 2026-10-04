# Template File Filter

The `siteorigin_widgets_template_file_{$id_base}` filter changes the template file that renders a widget, where `{$id_base}` is the widget's base ID. The Widgets Bundle uses `tpl/default.php` in the widget's folder unless the filter returns another path. The path must point to an existing file that ends in `.php`.

The filter can choose a template from a widget setting, as [Extending Existing Widgets](../../getting-started/extending-existing-widgets.md) shows.

```php
function mytheme_button_template_file( $filename, $instance, $widget ) {
	if ( ! empty( $instance['design']['theme'] ) && $instance['design']['theme'] == 'test' ) {
		// This option works for plugins.
		$filename = plugin_dir_path( __FILE__ ) . 'tpl/button.php';

		// For a theme, use this line instead.
		// $filename = get_stylesheet_directory() . '/tpl/button.php';
	}
	return $filename;
}
add_filter( 'siteorigin_widgets_template_file_sow-button', 'mytheme_button_template_file', 10, 3 );
```
