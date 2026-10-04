# Modifying Forms

You build most forms with the standard form array that [Form Fields](./form-fields.md) describes. A form can also change after you declare it, for example to extend a Widgets Bundle widget or to add data to your own widget's form only when the form is needed.

## Modifying a Form Inside a Widget

The `modify_form()` method of `SiteOrigin_Widget` returns the form array unchanged. Override it in your class to change the form:

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here

	function modify_form( $form ) {
		// We can modify this $form array however we want
		$form['test_field'] = array(
			'type'  => 'text',
			'label' => __( 'Test Field', 'siteorigin-docs' ),
		);
		return $form;
	}
}
```

`modify_form()` suits data that's expensive to load, such as a `select` field with several hundred options. Options in the main form array load into memory on every page load. The Widgets Bundle calls `modify_form()` only when it needs the form, so this example loads the options from a file only then. `modify_form()` runs when a user edits the widget only if the widget declares its form with `get_widget_form()`, as [Creating a Widget](../getting-started/creating-a-widget.md) shows.

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here

	function modify_form( $form ) {
		$form['my_field']['options'] = include __DIR__ . '/data-file.php';
		return $form;
	}
}
```

## Using a WordPress Filter

The Widgets Bundle passes every form array through two filters before it uses the form, so you can change a form from outside the widget:

```php
$form_options = apply_filters( 'siteorigin_widgets_form_options', $form_options, $this );
$form_options = apply_filters( 'siteorigin_widgets_form_options_' . $this->id_base, $form_options, $this );
```

`siteorigin_widgets_form_options` runs for every widget, so check that its second argument is the widget you want to change. `siteorigin_widgets_form_options_{$id_base}` runs for one widget only:

```php
function mytheme_filter_widget_form( $form_options, $widget ) {
	// This first line isn't necessary, but it's here for demonstration.
	if ( get_class( $widget ) != 'SiteOrigin_Widget_Button_Widget' ) {
		return $form_options;
	}

	if ( ! empty( $form_options['design']['fields']['theme']['options'] ) ) {
		$form_options['design']['fields']['theme']['options']['test'] = __( 'Test Style', 'siteorigin-docs' );
	}

	return $form_options;
}
add_filter( 'siteorigin_widgets_form_options_sow-button', 'mytheme_filter_widget_form', 10, 2 );
```

## Modifying a Child Widget's Form

A child widget is a widget inside the form of another widget, as [Child Widgets](./child-widgets.md) explains. The Call To Action Widget, for example, includes the Button Widget for its button. The Call To Action Widget aligns the button itself, so it removes the Button Widget's alignment fields with `modify_child_widget_form()`:

```php
class SiteOrigin_Widget_Cta_Widget extends SiteOrigin_Widget {
	// Everything else goes here.

	function modify_child_widget_form( $child_widget_form, $child_widget ) {
		// We could also check $child_widget if we're including different types of child widgets.
		unset( $child_widget_form['design']['fields']['align'] );
		unset( $child_widget_form['design']['fields']['mobile_align'] );

		return $child_widget_form;
	}
}
```

The parent widget changes the child widget's form only inside the parent, and the Button Widget's own form stays the same everywhere else.
