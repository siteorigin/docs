# Form Options Filter

The `siteorigin_widgets_form_options_{$id_base}` filter changes the fields of a widget's form, where `{$id_base}` is the widget's base ID. Use it to add fields to the Widgets Bundle's widgets or to your own, for example to add settings in a premium version of your plugin. The filter can change existing fields or add new ones. `siteorigin_widgets_form_options` changes the form of every widget, runs first and takes the same arguments. [Modifying Forms](../../form-building/modifying-forms.md) has more examples.

```php
function mytheme_filter_widget_form( $form_options, $widget ) {
	if ( ! empty( $form_options['design']['fields']['theme']['options'] ) ) {
		$form_options['design']['fields']['theme']['options']['test'] = __( 'Test Style', 'your-text-domain' );
	}
	return $form_options;
}
add_filter( 'siteorigin_widgets_form_options_sow-button', 'mytheme_filter_widget_form', 10, 2 );
```
