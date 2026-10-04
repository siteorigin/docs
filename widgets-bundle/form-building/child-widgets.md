# Child Widgets

Including a **child widget** (embedding one widget's form and output inside another widget) lets you reuse existing widget functionality without duplicating form fields or front end code. A common example is the Call-to-Action widget, which embeds the Button widget instead of recreating every button field.

## Defining a `widget` Form Field

Add a field of **`type` => `widget`** to your widget's `$form_options` array and specify the fully-qualified PHP class name of the widget you want to embed.

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

Make sure the child widget class is available **before** the parent form is built. For Widgets Bundle widgets, do this in your widget's `initialize()` method:

```php
if ( ! class_exists( 'SiteOrigin_Widget_Button_Widget' ) ) {
	SiteOrigin_Widgets_Bundle::single()->include_widget( 'button' );
}
```

## Rendering the Child Widget on the Front-End

The child widget's instance data is stored under the same key you used (`button_field` in this example). To output the widget inside your template:

### Using `sub_widget()`

Inside a widget template, `$this` is your widget, so you can call its `sub_widget( $class, $args, $instance, $return = false )` method. It clears `before_widget` and `after_widget`, and returns the HTML when `$return` is `true`. The Call To Action Widget renders its button this way.

```php
<?php $this->sub_widget( 'SiteOrigin_Widget_Button_Widget', $args, $instance['button_field'] ); ?>
```

### Using `$wp_widget_factory`

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

### Using `the_widget()`

```php
the_widget( 'SiteOrigin_Widget_Button_Widget', $instance['button_field'] );
```

> **Note:** `the_widget()` adds a default `<div class="widget">` wrapper. Use `$wp_widget_factory` if you need full control over the markup.

## Customizing the Child Widget Form

Sometimes the child widget has fields that are irrelevant in your parent widget. You can remove or tweak these fields in two ways.

### Overriding `modify_child_widget_form()`

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

### Using a `form_filter` Callback

The callback receives one argument, the child widget's form array.

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

## Examples in Core Widgets

| Parent Widget  | Child Widget(s)     | File                                  |
| -------------- | ------------------- | ------------------------------------- |
| Call-to-Action | Button              | `widgets/cta/cta.php`                 |
| Button Grid    | Repeater of Buttons | `widgets/button-grid/button-grid.php` |

Browse these files in the Widgets Bundle source to see working implementations.
