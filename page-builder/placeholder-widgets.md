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
			'description' => __('My custom widget description', 'mytheme'),
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

There's an optional `'plugin'` attribute in the widget array. This tells Page Builder where it can download and install the plugin from, if it's hosted on the WordPress.org directory.

### Rendering Missing Forms and Widgets

Page Builder gives you a few filters to deal with placeholder widgets. These are `siteorigin_panels_missing_widget_form`, `siteorigin_panels_missing_widget` and `siteorigin_panels_widget_object`.

#### Missing Widget Form

If Page Builder needs to render a form that doesn't exist, it'll pass the form HTML through the `` filter. It does this using the following form.

```
apply_filters('siteorigin_panels_missing_widget_form', $form, $widget, $instance);
```

Where `$form` is the current form HTML, `$widget` is the widget class and `$instance` is the widget instance. So, if you want to filter the HTML in your theme or plugin, you'll create a custom filter as follows.

```
function mytheme_filter_missing_widget_form($form, $widget, $instance){
	if( $widget === 'MyWidget_Class') {
		// This is our widget
		$form = mytheme_get_custom_missing_form($widget, $instance);
	}
	
	return $form;
}
add_filter('siteorigin_panels_missing_widget_form', 'mytheme_filter_missing_widget_form');
```

You can get create here as to what you do with the widget form. You could go as far as creating the entire form.

#### Missing Widget Content

Page Builder also lets you create the frontend content for a widget using the `siteorigin_panels_missing_widget` filter. If Page Builder ever tries to render a form but there's no widget to do the rendering, it'll pass an empty string through this filter.

```
echo apply_filters('siteorigin_panels_missing_widget', '', $widget, $args , $instance);
```

Where the first argument is the empy string for you to populate, `$widget` is the widget class, `$args` are the widget arguments and `$instance` is the widget instance.

Here's an example of using a custom function to render a missing widget.

```
function mytheme_render_missing_widget($html, $widget, $args, $instance){
	if( $widget === 'MyWidget_Class') {
		$html = '';
		$html .= $args['before_widget'] . $args['before_title'] . $instance['title'] . $args['after_title'];
		$html .= ' ... ' // This is for more detailed form content.
		$html .= $args['after_widget'];
	}
	
	return $html;
}
add_filter('siteorigin_panels_missing_widget', 'mytheme_render_missing_widget');
```

#### Missing Widget Object

You could also simplify the whole process and just create an placeholder widget object. This widget should be of a class that extends the `WP_Widget` class.

This is how that filter is called.

```
$the_widget = apply_filters( 'siteorigin_panels_widget_object', $the_widget, $widget );
```

Where `$the_widget` is the widget object and `$widget` is the class name of the widget.

Here's an example of how you'd implement that.

```
class My_Placeholder_Widget extends WP_Widget {
	// Add all your widget stuff here.
}

function mytheme_filter_widget_object($the_widget, $widget) {
	if(empty($the_widget) && $widget == 'My_Placeholder_Widget') {
		$the_widget = new $widget();
	}
	
	return $the_widget;
}
add_filter('siteorigin_panels_widget_object', 'mytheme_filter_widget_object');
```