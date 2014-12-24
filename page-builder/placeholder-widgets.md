# Placeholder Widgets

Page Builder supports the concept of placeholder widgets. These are widgets that your user might not have installed yet, but you'd like to encourage them to install.

### Adding Widgets

Page Builder filters all widgets in the **Add Widget** dialog though the `siteorigin_panels_widgets` filter before displaying them. The following filter adds widgets that don't exist into the `$widgets` array.

```php
function mytheme_recommended_widgets($widgets){
	if( empty(widgets['My_Custom_Widget']) ){
		$widgets['My_Custom_Widget'] = array(
			'class' => 'My_Custom_Widget',
			'title' => __('Custom Widget', 'siteorigin-panels'),
			'description' => __('My custom widget description', 'siteorigin-panels'),
			'installed' => false,
			'plugin' => array(
				'name' => __('Custom Widget Plugin', 'siteorigin-panels'),
				'slug' => 'plugin-slug'
			),
			'groups' => array('recommended'),
			'icon' => 'dashicons dashicons-edit',
		);
	}
	
	return $widgets;
}
add_filter('siteorigin_widgets', 'mytheme_recommended_widgets');
```
