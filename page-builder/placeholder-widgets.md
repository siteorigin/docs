# Placeholder Widgets

A placeholder widget appears in the **Add Widget** dialog before its plugin is installed, so you can recommend widgets for your users to install.

## Adding a Placeholder Widget

Page Builder passes every widget in the **Add Widget** dialog through the `siteorigin_panels_widgets` filter. This example adds a widget that isn't installed:

```php
function mytheme_recommended_widgets($widgets){
	if( empty($widgets['My_Custom_Widget']) ){
		$widgets['My_Custom_Widget'] = array(
			'class' => 'My_Custom_Widget',
			'title' => __('Custom Widget', 'mytheme'),
			'description' => __('My custom widget description', 'mytheme'),
			'installed' => false,
			'plugin' => array(
				'name' => __('Custom Widget Plugin', 'mytheme'),
				'slug' => 'plugin-slug'
			),
			'groups' => array('recommended'),
			'icon' => 'dashicons dashicons-edit',
		);
	}
	
	return $widgets;
}
add_filter('siteorigin_panels_widgets', 'mytheme_recommended_widgets');
```

The optional `plugin` key names the plugin that holds the widget. If the plugin is on the WordPress.org plugin directory, Page Builder uses the `slug` to install it.

Page Builder caches the widget list in the `siteorigin_panels_widgets` transient for 10 minutes. Delete the transient while you develop to see your changes right away.

Page Builder adds placeholders for the Widgets Bundle widgets itself. To remove them, disable **Recommended Widgets** in the Page Builder settings, or set `recommended-widgets` to `false` in your theme support arguments, as [Theme Integration](theme-integration.md) describes.

## Rendering Missing Forms and Widgets

Three filters handle a widget whose class doesn't exist: `siteorigin_panels_missing_widget_form`, `siteorigin_panels_missing_widget` and `siteorigin_panels_widget_object`.

### Missing Widget Form

When Page Builder renders the form of a widget whose class doesn't exist, it passes the form HTML through the `siteorigin_panels_missing_widget_form` filter:

```php
apply_filters('siteorigin_panels_missing_widget_form', $form, $widget, $instance);
```

`$form` is the form HTML, `$widget` is the widget's class name and `$instance` is the widget's instance. This example replaces the form of one widget with your own:

```php
function mytheme_filter_missing_widget_form($form, $widget, $instance){
	if( $widget === 'MyWidget_Class') {
		// This is our widget
		$form = mytheme_get_custom_missing_form($widget, $instance);
	}
	
	return $form;
}
add_filter('siteorigin_panels_missing_widget_form', 'mytheme_filter_missing_widget_form', 10, 3);
```

### Missing Widget Content

When Page Builder renders a widget whose class doesn't exist, it passes the widget's `before_widget` and `after_widget` HTML through the `siteorigin_panels_missing_widget` filter, so you can output the widget's content yourself:

```php
echo apply_filters('siteorigin_panels_missing_widget', $args['before_widget'] . $args['after_widget'], $widget, $args, $instance);
```

The first argument is the HTML to replace, `$widget` is the widget's class name, `$args` holds the widget arguments and `$instance` is the widget's instance. This example renders a missing widget's title:

```php
function mytheme_render_missing_widget($html, $widget, $args, $instance){
	if( $widget === 'MyWidget_Class') {
		$html = '';
		$title = isset( $instance['title'] ) ? $instance['title'] : '';
		$html .= $args['before_widget'] . $args['before_title'] . esc_html( $title ) . $args['after_title'];
		$html .= ' ... '; // This is for more detailed widget content.
		$html .= $args['after_widget'];
	}
	
	return $html;
}
add_filter('siteorigin_panels_missing_widget', 'mytheme_render_missing_widget', 10, 4);
```

### Placeholder Widget Object

The `siteorigin_panels_widget_object` filter replaces the missing widget with a widget object of your own, which handles both the form and the content. The object's class must extend `WP_Widget`. Page Builder calls the filter like this:

```php
$the_widget = apply_filters( 'siteorigin_panels_widget_object', $the_widget, $widget );
```

`$the_widget` is the widget object and `$widget` is the widget's class name. When Page Builder renders the widget on the front end, it also passes the widget's instance as a third argument. This example creates the placeholder widget object:

```php
class My_Placeholder_Widget extends WP_Widget {
	// Add all your widget stuff here.
}

function mytheme_filter_widget_object($the_widget, $widget) {
	if(empty($the_widget) && $widget == 'My_Placeholder_Widget') {
		$the_widget = new $widget();
	}
	
	return $the_widget;
}
add_filter('siteorigin_panels_widget_object', 'mytheme_filter_widget_object', 10, 2);
```
