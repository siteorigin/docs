# Initialize Widget Action

The `siteorigin_widgets_initialize_widget_{$id_base}` action runs right after the Widgets Bundle initializes a widget, so you can make changes that need the initialized widget. `{$id_base}` is the widget's base ID, so the action runs for one widget at a time, and no version of it runs for every widget. The action runs inside the widget's constructor and passes the widget object, so it runs each time a widget object is created.

```php
/**
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_after_button_init( $widget ) {
    // Add your custom code here
}
add_action( 'siteorigin_widgets_initialize_widget_sow-button', 'wbe_after_button_init' );
```
