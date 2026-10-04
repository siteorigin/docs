# Initialize Widget Action

This action is triggered right after the core Widgets Bundle has run all its initialization actions for the specified widget. The intention is that this will allow you to additional adjustments that couldn't necessarily be done until after the widget been inititalization.

This action is widget specific, so it's not possible to automatically target all widgets by default. The syntax for the action name is: `'siteorigin_widgets_initialize_widget_' . $this->id_base`. The action fires inside the widget constructor and passes the widget object, so it runs each time a widget object is created.

```php
/**
 * @param SiteOrigin_Widget $widget The widget object.
 */
function wbe_after_button_init( $widget ) {
    // Add your custom code here
}
add_action( 'siteorigin_widgets_initialize_widget_sow-button', 'wbe_after_button_init' );
```
