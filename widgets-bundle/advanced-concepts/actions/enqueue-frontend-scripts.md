# Enqueue frontend scripts action

This action gives you a chance to enqueue any additional frontend scripts and styles for a widget. If you need to enqueue scripts for your own widgets, you can read about that in our getting started section on [initializing a widget](../../getting-started/initializing-a-widget.md). This action is mainly for enqueuing additional scripts and styles for a widget that you're extending.

This action has the form `'siteorigin_widgets_enqueue_frontend_scripts_' . $this->id_base` and `'siteorigin_widgets_enqueue_frontend_scripts'`. The version with the id_base is designed to target a specific widget.

```php
/**
 * @param array $instance The button instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_enqueue_button_scripts( $instance, $widget ){
	wp_enqueue_script( ... );
	wp_enqueue_style( ... );
}
add_action('siteorigin_widgets_enqueue_frontend_scripts_ sow-button', wbe_enqueue_button_scripts, 10, 2);
```