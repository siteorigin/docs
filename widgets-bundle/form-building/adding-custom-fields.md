# Adding Custom Fields

You can build your own form fields for Widgets Bundle widgets by extending our field classes. The full example code is in the [custom-fields folder](https://github.com/siteorigin/so-dev-examples/tree/develop/extend-widgets-bundle/custom-fields/) of our so-dev-examples repository.

## Field Class Names

A field's class name is a class prefix followed by the field type. The field type is split on hyphens, and each part is capitalized and joined with underscores. The Widgets Bundle's text field, for example, has the type `text` and the prefix `SiteOrigin_Widget_Field_`, so its class is `SiteOrigin_Widget_Field_Text`. A `better-text` field type with the prefix `My_Custom_Field_` has the class `My_Custom_Field_Better_Text`.

## Adding a Class Prefix

Give your field classes a prefix of your own, so their names don't clash with other classes. The Widgets Bundle needs your prefix to load and create your field classes. You can add more than one prefix, and one is enough.

### Example: Adding a Class Prefix

```php
function my_custom_fields_class_prefixes( $class_prefixes ) {
	$class_prefixes[] = 'My_Custom_Field_';
	return $class_prefixes;
}
add_filter( 'siteorigin_widgets_field_class_prefixes', 'my_custom_fields_class_prefixes' );
```

## Adding a Class Path

The Widgets Bundle also needs the folder that holds your field class files, so it can load them. Add the folder with the `siteorigin_widgets_field_class_paths` filter.

### Example: Adding a Class Path

```php
function my_custom_fields_class_paths( $class_paths ) {
	$class_paths[] = plugin_dir_path( __FILE__ ) . 'custom-fields/';
	return $class_paths;
}
add_filter( 'siteorigin_widgets_field_class_paths', 'my_custom_fields_class_paths' );
```

## Implementing a Custom Field

Extend one of the existing field classes and override the methods you want to change. A class that extends `SiteOrigin_Widget_Field_Base` directly must implement at least `render_field()` and `sanitize_field_input()`, which are enough to render the field and save its input.

### File and Class Names

Name your class with your prefix and the field type, as described above, and name its file `{field type}.class.php` in one of your class paths. For a `taxonomylist` field type with the class path `my_custom_fields/` and the prefix `My_Custom_Field_`, create `my_custom_fields/taxonomylist.class.php` and define the `My_Custom_Field_Taxonomylist` class in it. The `My_Custom_Field_Better_Text` class for the `better-text` field type goes in `better-text.class.php`.

### Extending `SiteOrigin_Widget_Field_Base`

The `SiteOrigin_Widget_Field_Base` abstract class does most of the work of a form field: it renders the field in the widget form and prepares the field's input to be saved. A class that extends it must implement two abstract methods, `render_field()` and `sanitize_field_input()`.

#### The `render_field()` Method

`render_field()` outputs your field's HTML in the widget form. It receives `$value`, the field's current value, which you escape right before output, and `$instance`. The Widgets Bundle passes an empty array as `$instance`, so override `render_before_field()` or `render_after_field()` to read other values from the widget instance, because they receive the full instance.

##### Example: `render_field()`

```php
protected function render_field( $value, $instance ) {
	?>
	<input type="text" class="siteorigin-widget-input" id="<?php echo esc_attr( $this->element_id ); ?>" name="<?php echo esc_attr( $this->element_name ); ?>"
		   value="<?php echo esc_attr( $value ); ?>"/>
	<?php
}
```

#### The `sanitize_field_input()` Method

`sanitize_field_input()` puts the input from the widget form in the format your field needs and removes unwanted characters. It receives `$value`, the raw input, and `$instance`, the widget instance. Use the WordPress sanitization functions to sanitize the value.

##### Example: `sanitize_field_input()`

```php
protected function sanitize_field_input( $value, $instance ) {
	$sanitized_value = sanitize_text_field( $value );
	return $sanitized_value;
}
```

#### Adding Properties

Your field can take its own options. Declare a property in your class, and the base field class sets it from the field option with the same name.

##### Example: Adding a Property

Declare the property in your class:

```php
class My_Custom_Field_Better_Text extends SiteOrigin_Widget_Field_Text {
	/**
	 * My custom property for doing custom things.
	 *
	 * @access protected
	 * @var mixed
	 */
	protected $my_property;
}
```

Then set the option with the same name when you use the field:

```php
array(
	'text' => array(
		'type' => 'better-text',
		'my_property' => 'This is my custom property value',
		'label' => __( 'A better text field.', 'siteorigin-docs' ),
		'default' => 'Some better text.'
	),
),
```

#### Rendering the Label

`SiteOrigin_Widget_Field_Base` renders the field's label with its `render_field_label()` method. Override `render_field_label()` to render the label yourself, or override `get_label_classes()` to add CSS classes to the default label, which is easier for subclasses to build on. Enqueue the stylesheet with your label classes, for example in the field's `enqueue_scripts()` method.

##### Example: Overriding `render_field_label()`

```php
protected function render_field_label( $value, $instance ) {
	?>
	<h1>My custom label rendering</h1>
	<?php
}
```

##### Example: Adding Label Classes

```php
protected function get_label_classes( $value, $instance ) {
	$label_classes = parent::get_label_classes( $value, $instance );
	$label_classes[] = 'additional-CSS-class';
	return $label_classes;
}
```

#### Rendering the Description

`SiteOrigin_Widget_Field_Base` renders the field's description with its `render_field_description()` method, which you can change the same way as the label.

#### Rendering Before and After the Field

`SiteOrigin_Widget_Field_Base` calls `render_before_field()` before the main rendering method and `render_after_field()` after it, so every field gets its label above and its description below. Override them to drop the label or the description, or to add your own HTML.

##### Example: Overriding `render_before_field()` and `render_after_field()`

This example renders the description after the label, above the field:

```php
protected function render_before_field( $value, $instance ) {
	// This is to keep the default label rendering behaviour.
	parent::render_before_field( $value, $instance );
	// Add custom rendering here.
	$this->render_field_description();
}

protected function render_after_field( $value, $instance ) {
	// Leave this blank so that the description is not rendered twice
}
```

#### The `sanitize_instance()` Method

A field that changes values in the widget instance other than its own input needs to sanitize the instance too. Override `sanitize_instance()`, which receives the widget instance and must return it.

#### The `get_related_instance_keys()` Method

When a user saves a widget, the Widgets Bundle removes every instance key that isn't a form field. If your field saves a value under a key of its own, such as `{field name}_unit`, override `get_related_instance_keys()` and return an array of those keys, so the Widgets Bundle keeps them. The Widgets Bundle doesn't sanitize these keys, so sanitize them in `sanitize_instance()`. The media field does both for its fallback URL key.

```php
private function get_unit_key() {
	$name = $this->base_name;

	// Inside a section or repeater, the base name includes the parent names. Keep the last part.
	if ( strpos( $name, '][' ) !== false ) {
		$name = substr( $name, strrpos( $name, '][' ) + 2 );
	}

	return $name . '_unit';
}

public function get_related_instance_keys() {
	return array( $this->get_unit_key() );
}

public function sanitize_instance( $instance ) {
	$unit_key = $this->get_unit_key();

	if ( isset( $instance[ $unit_key ] ) ) {
		$instance[ $unit_key ] = in_array( $instance[ $unit_key ], array( 'px', 'em', '%' ), true ) ? $instance[ $unit_key ] : 'px';
	}

	return $instance;
}
```

#### Default Options and Initialization

Override `get_default_options()` to return default values for your field's properties, which options in the form array override. Override `initialize()` to run code after the options are set.

#### Scripts and Styles

Override `enqueue_scripts()` to enqueue the field's JavaScript and CSS. The widget calls the method for each field as it renders the form. To set up the field in JavaScript, listen for the `sowsetupformfield` event on the field's wrapper, which has the class `siteorigin-widget-field-type-{field type}`.

```javascript
jQuery( document ).on( 'sowsetupformfield', '.siteorigin-widget-field-type-better-text', function() {
	var $field = jQuery( this );
	// Set up the field here.
} );
```

#### JavaScript Variables

Override `get_javascript_variables()` to pass values from your field to its JavaScript. The widget calls the method as it renders its form and passes the values to the browser in the global `sow_field_javascript_variables` object, keyed by widget class and then field name: `sow_field_javascript_variables[ widgetClass ][ fieldName ]`.

### Using a Custom Field

Use your custom field in a widget like any other field:

```php
$form_options = array(
	'text' => array(
		'type' => 'better-text',
		'my_property' => 'This is my custom property value',
		'label' => __( 'A better text field.', 'siteorigin-docs' ),
		'description' => __( 'A description for my custom text field.', 'siteorigin-docs' ),
		'default' => 'Some better text.'
	),
);
```
