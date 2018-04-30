# Initialize widget action

This action is triggered right after the core Widgets Bundle has run all its initialization actions for the specified widget. The intention is that this will allow you to additional adjustments that couldn't necessarily be done until after the widget been inititalization.

This action is widget specific, so it's not possible to automatically target all widgets by default. The syntax for the action name is: `'siteorigin_widgets_initialize_widget_' . $this->id_base. 

```php
/**
 * @param array $instance The button instance.
 */
function wbe_custom_code_after_widget_init( $instance ){
    // Add your custom code here
}
add_action( 'siteorigin_widgets_initialize_widget_sow-button', 'wbe_custom_code_after_widget_init' );
```
