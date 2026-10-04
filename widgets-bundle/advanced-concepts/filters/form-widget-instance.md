# Form Widget Instance Filter

The `siteorigin_widgets_form_instance_{$id_base}` filter changes a widget's instance before the form shows it, where `{$id_base}` is the widget's base ID. Use it, for example, to convert a widget's old data after you change the structure of its form. For a widget you've created, use `modify_form()` instead, as [Changing Form Structure](../../tutorials/changing-form-structure.md) explains.

```php
function wbe_modify_form_instance( $instance, $widget ) {
    // We can modify the instance here.
    $instance['text'] = 'Never Change!';
    return $instance;
}
add_filter( 'siteorigin_widgets_form_instance_sow-button', 'wbe_modify_form_instance', 10, 2);
```
