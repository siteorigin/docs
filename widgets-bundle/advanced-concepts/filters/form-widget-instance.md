# Form widget instance filter

This filter allows you to modify the widget instance before its rendered by the form. There are a few cases where you might want to use this. One example is when you change the structure of your widget form and you want to convert old widget data to new widget data.

If you're doing that with a widget that you've created, then you would be better off using using modify_form. We've created a guide on [changing form structure](../../tutorials/changing-form-structure.md).

This filter takes the form `'siteorigin_widgets_form_instance_' . $this->id_base`, where id_base is the ID of the widget.

```php
function wbe_modify_form_instance( $instance, $this ) {
	// We can modify the instance here.
	$instance['text'] = 'Never Change!';
	return $instance;
}
add_filter( 'siteorigin_widgets_form_instance_sow-button', 'wbe_modify_form_instance', 10, 2);
```

