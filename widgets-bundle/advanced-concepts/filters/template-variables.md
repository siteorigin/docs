# Template variables filter

To understand this filter, it's best to look at the context in which it runs. The widgets bundle passes the widget instance to the `get_template_variables` function, which is the function your plugin should be overriding to return an array of vairables that'll be passed through `extract` and made available to the template files.

```php
$template_vars = $this->get_template_variables($instance, $args);
$template_vars = apply_filters( 'siteorigin_widgets_template_variables_' . $this->id_base, $template_vars, $instance, $args, $this );
extract( $template_vars );
```

This filter mainly comes in useful if you want to modify the template variables of another widget. You could technically just modify the frontend instance, but this filter gives you a way to modify the variables in a way that doesn't change the instance for the LESS templates.

The filter takes the form `'siteorigin_widgets_template_variables_' . $this->id_base`. The Widgets Bundle passes it 4 arguments.

```php
/**
 * @param array $template_vars The Template variables array
 * @param array $instance The widget instance
 * @param array $args The widget instance
 * @param SiteOrigin_Widget $widget The main widget instance.
 */
function wbe_filter_template_variables( $template_vars, $instance, $args, $widget ){
	// Strictly this isn't correct, but we'll use it as an example
	$template_vars['text'] = __( $instance['text'], 'test-domain' );
}
add_filter( 'siteorigin_widgets_template_variables_sow-button', 'wbe_filter_template_variables', 10, 4 );
```