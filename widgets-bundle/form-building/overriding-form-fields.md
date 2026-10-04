# Overriding Form Fields

The `siteorigin_widgets_field_registered_class_paths` filter replaces a Widgets Bundle form field with your own class. Add a folder of field files to the filter, and the Widgets Bundle loads a file from your folder in place of a base field file with the same name and class. This example adds a `fields` folder in your plugin:

```php
function siteorigin_load_custom_form_fields( $class_paths ) {
	array_unshift( $class_paths['base'], plugin_dir_path( __FILE__ ) . 'fields/' );
	return $class_paths;
}
add_filter( 'siteorigin_widgets_field_registered_class_paths', 'siteorigin_load_custom_form_fields' );
```

The snippet works as written in a plugin. The filter runs on `init`, so add it before `init`, for example when your plugin loads.

To replace the TinyMCE field, create the `fields` folder next to the file with the snippet. Add a file named `tinymce.class.php`, the same name as the base TinyMCE field's file, with this class:

```php
<?php
class SiteOrigin_Widget_Field_TinyMCE extends SiteOrigin_Widget_Field_Text_Input_Base {
	protected function render_field( $value, $instance ) {
		echo 'Field was successfully replaced.';
	}
}
```

Every TinyMCE field now shows "Field was successfully replaced." in place of the editor. Download the [example plugin](https://siteorigin.com/wp-content/uploads/2021/08/siteorigin-override-form-field.zip) for the full code and folder structure.
