# Form options filter

This is an incredibly useful filter for changing the fields of an existing widget form. You can use this to enhance the widgets currently in Widgets Bundle or you can enhance widgets you've added. Maybe you want to add enhanced functionality in a premium version of your plugin.

This filter allows you to edit existing fields or add your own new ones. See the [form modification](../../form-building/modifying-forms.md) doc for more details.

```php
function mytheme_filter_widget_form($form_options, $widget){
    if( !empty($form_options['design']['fields']['theme']['options']) ) {
        $form_options['design']['fields']['theme']['options']['test'] = __('Test Style', 'mytheme');
    }
}
add_filter('siteorigin_widgets_form_options_sow-button', 'mytheme_filter_widget_form', 10, 2)
```