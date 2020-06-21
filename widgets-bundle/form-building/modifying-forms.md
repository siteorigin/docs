# Modifying forms

For the most part, you'll be creating your forms using the standard [forms array](./form-fields.md). There are times, however, that you'll want to modify the form at a later stage. There are a few use cases for this. You might want to extend the functionality of an existing widget in the Widgets Bundle, or extend the functionality of a widget you've created.

## Modifying a form within a widget

The `SiteOrigin_Widget` class has a placeholder function called `modify_form`. By default, this function just returns the $form array unchanged, but you can override this in your class to do whatever you like.

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here

	function modify_form( $form ) {
		// We can modify this $form array however we want
		$form['test_field'] = array(
			'type' => 'text',
			'label' => __('Test Field', 'my-theme'),
		);
		return $form;
	}
}
```

So when would you want to use this? One example is to late-add data to your form array. Maybe you have a `select` field with several hundred options that you want to add to the `options`. Adding this in the main form array means you're loading all the options into memory with every page load.

The `modify_form` function is only ever called when the Widgets Bundle needs the form. So you can load the large options array from a file and not  use memory unnecessarily on every request.

```php
class MyCustomWidget extends SiteOrigin_Widget {
    // We're leaving out all the setup code here
    
    function modify_form( $form ) {
        $form['my_field']['options'] = include( 'data-file.php' );
        return $form;
    }
}
```

## Using a WordPress filter

The Widgets Bundle also passes your form array through a few filters before ever using it. This allows you to modify the array outside the widget itself.

```php
$form_options = apply_filters( 'siteorigin_widgets_form_options', $form_options, $this );
$form_options = apply_filters( 'siteorigin_widgets_form_options_' . $this->id_base, $form_options, $this );
```

Both of these are quite easy to hook into. If you're using the `siteorigin_widgets_form_options` filter, then your function should check that the 2nd argument is the class you want to filter. The other filter will only run for one specific widget type.

```php
function mytheme_filter_widget_form($form_options, $widget){
    // This first line isn't necessary, but it's here for demonstration.
    if( get_class($widget) != 'SiteOrigin_Widget_Button_Widget' ) return $form_options;
    
    if( !empty($form_options['design']['fields']['theme']['options']) ) {
        $form_options['design']['fields']['theme']['options']['test'] = __('Test Style', 'mytheme');
    }
    return $form_options;
}
add_filter('siteorigin_widgets_form_options_sow-button', 'mytheme_filter_widget_form', 10, 2);
```

## Modifying a child widget form

The Widgets Bundle has a concept of a child widget. This is a widget that's included in the form of another widget. The Call-to-action widget, for example, includes a button widget. Rather than completely recreate the button functionality in the Call-to-action widget, it just includes the button widget as a child widget field.

In this case though, there might be some fields in the child widget that aren't necessary. In the case of the Call-to-action widget, the alignment field of the Button widget isn't necessary because the CTA widget itself is handling this. In this case, we use `modify_child_widget_form` to remove that field.

```php
class SiteOrigin_Widget_Cta_widget extends SiteOrigin_Widget {
    // Everything else goes here

    function modify_child_widget_form($child_widget_form, $child_widget) {
        // We could also check $child_widget if we're including different types of child widgets
        unset( $child_widget_form['design']['fields']['align'] );
        return $child_widget_form;
    }
}
```

The most important thing to note here is that the CTA widget is the one that's filtering the button widget's form. This filtering function allows the parent CTA widget to decide exactly how it wants to handle the button widget without having an effect on the main button widget itself.
