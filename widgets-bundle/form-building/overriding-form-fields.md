# Overriding Form Fields.

It's possible to completely override a form field by passing a directory to the `siteorigin_widgets_field_registered_class_paths` filter that contains a file named exactly the same as the base form field with the same class set. For example, if I wanted to override the TinyMCE field I would load a directory containing fields using the following PHP:

```php
function siteorigin_load_custom_form_fields( $class_paths ) {
	array_unshift( $class_paths['base'], plugin_dir_path( __FILE__ ) . 'fields/' );
	return $class_paths;
}
add_filter( 'siteorigin_widgets_field_registered_class_paths', 'siteorigin_load_custom_form_fields' );
```

The above snippet will only work in a plugin without modification.

Then create a directory in the same directory as the above PHP snippet called `fields`. In that directory please add a file called `tinymce.class.php` (the same filename as the base TinyMCE Form Field). Add the following PHP to that file:

```php
<?php
class SiteOrigin_Widget_Field_TinyMCE extends SiteOrigin_Widget_Field_Text_Input_Base {
	protected function render_field( $value, $instance ) {
		echo 'Field was successfully replaced.';
	}
}
```

This will now override the TinyMCE field with the message 'Field was successfully replaced'.

[Here's a reference plugin containing the above code with the outlined file structure](https://siteorigin.com/wp-content/uploads/2021/08/siteorigin-override-form-field.zip)
