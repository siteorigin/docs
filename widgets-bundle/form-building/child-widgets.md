# Child Widgets

A child widget is a widget whose form and output sit inside another widget, so you reuse its fields and front-end code without copying them. The Call To Action Widget, for example, embeds the Button Widget for its button.

## Adding a Widget Field

Add a field with the `widget` type to your widget's `$form_options`, and set `class` to the full PHP class name of the widget to embed:

```php
$form_options = array(
	'button_field' => array(
		'type'  => 'widget',
		'label' => __( 'Button', 'siteorigin-docs' ),
		'class' => 'SiteOrigin_Widget_Button_Widget',
		'hide'  => true, // Start collapsed (optional).
	),
);
```

### Loading the Child Widget Class

The child widget's class must exist before the Widgets Bundle builds the parent's form. For a Widgets Bundle widget, load the class in your widget's `initialize()` method:

```php
if ( ! class_exists( 'SiteOrigin_Widget_Button_Widget' ) ) {
	SiteOrigin_Widgets_Bundle::single()->include_widget( 'button' );
}
```

## Rendering the Child Widget

The child widget's instance is saved under the field's key, `button_field` in this example. Output the child widget in your template in one of three ways.

### With `sub_widget()`

In a widget template, `$this` is your widget, so call its `sub_widget( $class, $args, $instance, $return = false )` method. The method clears `before_widget` and `after_widget`, and returns the HTML when `$return` is `true`. The Call To Action Widget renders its button this way:

```php
<?php $this->sub_widget( 'SiteOrigin_Widget_Button_Widget', $args, $instance['button_field'] ); ?>
```

### With `$wp_widget_factory`

```php
global $wp_widget_factory;

if (
	! empty( $instance['button_field'] ) &&
	! empty( $wp_widget_factory->widgets['SiteOrigin_Widget_Button_Widget'] )
) {
	// Render the child widget without extra wrapper markup
	$wp_widget_factory->widgets['SiteOrigin_Widget_Button_Widget']->widget( array(), $instance['button_field'] );
}
```

### With `the_widget()`

```php
the_widget( 'SiteOrigin_Widget_Button_Widget', $instance['button_field'] );
```

`the_widget()` wraps the widget in a `<div class="widget">`. Use `$wp_widget_factory` for full control over the markup.

## Changing the Child Widget's Form

The child widget's form can include fields that your parent widget doesn't need. Remove or change them in one of two ways.

### With `modify_child_widget_form()`

Override `modify_child_widget_form()` in your widget:

```php
class My_Parent_Widget extends SiteOrigin_Widget {
	function modify_child_widget_form( $child_form, $child_widget ) {
		// Remove alignment controls from the Button widget.
		unset( $child_form['design']['fields']['align'] );
		unset( $child_form['design']['fields']['mobile_align'] );
		return $child_form;
	}
}
```

### With a `form_filter` Callback

Set a `form_filter` callback on the field. The callback receives the child widget's form array:

```php
class My_Parent_Widget extends SiteOrigin_Widget {
	public function get_widget_form() {
		return array(
			'button_field' => array(
				'type'        => 'widget',
				'class'       => 'SiteOrigin_Widget_Button_Widget',
				'form_filter' => array( $this, 'filter_child_form' ),
			),
		);
	}

	public function filter_child_form( $form ) {
		unset( $form['design']['fields']['align'] );
		return $form;
	}
}
```

## Examples in the Widgets Bundle

| Parent widget | Child widget | File |
| --- | --- | --- |
| Call To Action Widget | Button Widget | `widgets/cta/cta.php` |
| Button Grid Widget | Button Widget, in a repeater | `widgets/button-grid/button-grid.php` |
