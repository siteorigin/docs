# Enqueue Admin Scripts Action

The `siteorigin_widgets_enqueue_admin_scripts_{$id_base}` action enqueues extra admin scripts and styles for one widget, where `{$id_base}` is the widget's base ID. Use it for a widget you're extending, and enqueue scripts for your own widgets as [Initializing a Widget](../../getting-started/initializing-a-widget.md) describes. The action passes one argument, the widget object.

```php
/**
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_enqueue_button_admin_scripts( $widget ) {
	wp_enqueue_script( ... );
	wp_enqueue_style( ... );
}
add_action( 'siteorigin_widgets_enqueue_admin_scripts_sow-button', 'wbe_enqueue_button_admin_scripts' );
```
