# Enqueue Frontend Scripts Action

The `siteorigin_widgets_enqueue_frontend_scripts_{$id_base}` action enqueues extra front-end scripts and styles for one widget, where `{$id_base}` is the widget's base ID. Use it for a widget you're extending, and enqueue scripts for your own widgets as [Initializing a Widget](../../getting-started/initializing-a-widget.md) describes. The action runs each time the widget renders, except in a Page Builder preview.

```php
/**
 * @param array $instance The button instance.
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_enqueue_button_frontend_scripts( $instance, $widget ) {
	wp_enqueue_script( ... );
	wp_enqueue_style( ... );
}
add_action( 'siteorigin_widgets_enqueue_frontend_scripts_sow-button', 'wbe_enqueue_button_frontend_scripts', 10, 2 );
```
