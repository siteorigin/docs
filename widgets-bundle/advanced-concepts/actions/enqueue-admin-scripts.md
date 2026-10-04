# Enqueue Admin Scripts Action

This action gives you a chance to enqueue any additional admin scripts and styles for a widget. If you need to enqueue scripts for your custom widgets, you can read about that in our getting started section on [initializing a widget](../../getting-started/initializing-a-widget.md). This action is mainly for enqueuing additional scripts and styles for a widget that you're extending.

This action has the form `'siteorigin_widgets_enqueue_admin_scripts_' . $this->id_base`, so it targets a specific widget. It passes one argument, the widget object.

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
