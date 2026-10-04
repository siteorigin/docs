# Frontend Widget Instance Filter

The frontend widget instance filters change a widget's instance before the Widgets Bundle renders the widget. The Widgets Bundle passes the filtered instance to `get_less_variables()` and `get_template_variables()`, so the filters change the front end without changing the instance in the form or in the database.

`siteorigin_widgets_instance` runs for every widget, and `siteorigin_widgets_instance_{$id_base}` runs for one widget, where `{$id_base}` is the widget's base ID. Both pass two arguments, the instance and a copy of the widget object. This example changes the Button Widget:

```php
function wbe_filter_button_frontend_instance($instance, $widget){
    if ( ! empty( $instance['text'] ) && $instance['text'] === 'Download Now' ) {
        // We want to run any buttons that say download now through a custom function
        // wbe_modify_button_text is an example function
        $instance['text'] = wbe_modify_button_text( $instance['text'] );
    }
    
    return $instance;
}
add_filter('siteorigin_widgets_instance_sow-button', 'wbe_filter_button_frontend_instance', 10, 2);
```

`wbe_modify_button_text()` stands for your own function, such as a translation function that changes the button's text.
