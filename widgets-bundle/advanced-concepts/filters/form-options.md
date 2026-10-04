# Form Options Filter

The form filter is an incredibly useful filter for changing the fields of an existing widget form. You can use this to enhance the widgets currently in Widgets Bundle, or you can enhance widgets you've added. Maybe you want to add enhanced functionality in a premium version of your plugin.

This filter allows you to edit existing fields or add new ones. The filter has the form `'siteorigin_widgets_form_options_' . $this->id_base`. To change the form of every widget, use `siteorigin_widgets_form_options`, which runs first and takes the same arguments. See the [form modification](../../form-building/modifying-forms.md) doc for more details.

```php
function mytheme_filter_widget_form( $form_options, $widget ) {
	if ( ! empty( $form_options['design']['fields']['theme']['options'] ) ) {
		$form_options['design']['fields']['theme']['options']['test'] = __( 'Test Style', 'your-text-domain' );
	}
	return $form_options;
}
add_filter( 'siteorigin_widgets_form_options_sow-button', 'mytheme_filter_widget_form', 10, 2 );
```
