# Frontend widget instance filter

This filter allows you to modify the widget instance right before the widget is rendered. This is the version of the instance that's passed to the `get_less_variables` and `get_template_variables`. This filter allows you to midify the instance on the frontend without effecting the instance that's handled by the forms or stored in the database.

This filter has 2 versions. `'siteorigin_widgets_instance'` and `'siteorigin_widgets_instance_' . $this->id_base`. One targets all widgets and the other only targets a widget with a specific `id_base`. It gets 2 arguments, the first is the instance to be filterd and the second is a copy of the widget object.

This is an example of filtering the button widget.

```php
function wbe_filter_button_frontend_instance($instance, $widget){
	if( $instance['text'] == 'Download Now' ) {
		// We want to run any buttons that say download now through a custom function
		// wbe_modify_button_text is an example function
		$instance['text'] = wbe_modify_button_text( $instance['text'] );
	}
	
	return $instance;
}
add_filter('siteorigin_widgets_instance_sow-button', 'wbe_filter_button_frontend_instance', 10, 2);
```

The function `wbe_modify_button_text` doesn't actually exist, but you could replace it with something like a translation function, or maybe you want to add some sort of user specific tracking argument to the button URL.