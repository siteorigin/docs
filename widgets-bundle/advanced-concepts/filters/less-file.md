# Filter less file

This filter gives you a way to change the LESS file that is being used to create your widget. We use this filter in our section on [extending existing widgets](../../getting-started/extending-existing-widgets.md). This will give you the best idea of what this filter is for and how you'll likely be using it.

This filter has the form `'siteorigin_widgets_less_file_' . $this->id_base`, where id_base is the ID of the widget.

```php
function mytheme_button_less_file( $filename, $instance, $widget ){
	if( !empty($instance['design']['theme']) && $instance['design']['theme'] == 'test' ) {
		$filename = plugin_dir_path( __FILE__ ) . 'less/test.less';
		
		// And this one for themes
		$filename = get_stylesheet_dir() . '/less/test.less'; 
	}
	return $filename;
}
add_filter( 'siteorigin_widget_less_file_sow-button', 'mytheme_button_less_file', 10, 3 );
```