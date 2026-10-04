# Adding Custom Fields

We have made the form fields, used by SiteOrigin widgets, extendible so that you can easily create your own custom fields. There are a few steps involved, but each of them is fairly simple. You can see the example code in the so-dev-examples repository <a href="https://github.com/siteorigin/so-dev-examples/tree/develop/extend-widgets-bundle/custom-fields/" target="_blank">here</a>.

## Field Class Names
We "namespace" our classes by prefixing their names with some (hopefully unique) prefix. The full class name is the class prefix followed by the field type, with the field type split on hyphens, each part capitalized and the parts joined with underscores. For example, the basic text field in the Widgets Bundle has the type `text` and it is prefixed by `SiteOrigin_Widget_Field_`, so the resulting class name is `SiteOrigin_Widget_Field_Text`. A field type of `better-text` with the prefix `My_Custom_Field_` gives the class name `My_Custom_Field_Better_Text`.

## Adding Custom Field Class Prefixes
We encourage you to prefix your custom field class names to avoid conflicts with other class names. If you do this, the Widgets Bundle needs to know what your chosen prefix is, in order to autoload and instantiate your custom field classes. If you need to, you can add more than one prefix, but one is sufficient for the Widgets Bundle.

### Example - Adding Class Prefixes
```php
function my_custom_fields_class_prefixes( $class_prefixes ) {
	$class_prefixes[] = 'My_Custom_Field_';
	return $class_prefixes;
}
add_filter( 'siteorigin_widgets_field_class_prefixes', 'my_custom_fields_class_prefixes' );
```

## Adding Custom Field Class Paths
It is necessary for the Widgets Bundle to know which directory you custom field class files are kept in for the purpose of autoloading. You can add your class paths to the autoloader by adding the `siteorigin_widgets_field_class_paths` filter.

### Example - Adding Class Paths
```php
function my_custom_fields_class_paths( $class_paths ) {
	$class_paths[] = plugin_dir_path( __FILE__ ) . 'custom-fields/';
	return $class_paths;
}
add_filter( 'siteorigin_widgets_field_class_paths', 'my_custom_fields_class_paths' );
```
## Implementing a Custom Field
Implementing a custom field is as simple as extending one of the existing field classes and implementing or overriding at least the `render_field` and `sanitize_field_input` methods. There is much more that can be done, but this is all that is required to successfully render a custom field and save it's input.

### Filenames and Class Naming
For your field class to be loaded, you need to name your class according to the convention mentioned above. However the file itself must be named according to the convention `$field_type.class.php` and it must be placed in one of the class paths you added in the step above. For example, if you have a field type of `taxonomylist` with a custom class path of `my_custom_fields/` and a class prefix of `My_Custom_Field_`, you'd first create the file `my_custom_fields/taxonomylist.class.php` and then define the class `My_Custom_Field_Taxonomylist` inside it. The `My_Custom_Field_Better_Text` class for the `better-text` field type goes in `better-text.class.php`.

### Inheriting from SiteOrigin_Widget_Field_Base
The `SiteOrigin_Widget_Field_Base` abstract class handles most of the work required for the widget form fields. It contains various properties and methods which are used to render the field in the widget form and preparing input from the field for database persistence. When extending this class there are two abstract methods which must be implemented, namely, `render_field` and `sanitize_field_input`.

#### The `render_field` Method
`render_field` should output the HTML required for your custom field in the widget form. The method receives two arguments, `$value` and `$instance`. `$value` is the current value of the field for a specific instance of a widget form and should always be escaped just before output. The Widgets Bundle passes an empty array as `$instance` to `render_field`. To read other values from the widget instance, override `render_before_field` or `render_after_field`, which receive the full instance.

##### Example - `render_field` Implementation
```php
protected function render_field( $value, $instance ) {
	?>
	<input type="text" class="siteorigin-widget-input" id="<?php echo esc_attr( $this->element_id ); ?>" name="<?php echo esc_attr( $this->element_name ); ?>"
		   value="<?php echo esc_attr( $value ); ?>"/>
	<?php
}
```

#### The `sanitize_field_input` Method
`sanitize_field_input` should ensure that the input received from the widget form is in the desired format and any unwanted characters are removed. It receives two arguments, `$value`, which is the raw value of the field input, and `$instance`, the widget instance. Typically this value is sanitized using the built-in WordPress sanitization functions.

##### Example - `sanitize_field_input` Implementation
```php
protected function sanitize_field_input( $value, $instance ) {
	$sanitized_value = sanitize_text_field( $value );
	return $sanitized_value;
}
```

#### Adding Properties
You may wish to have additional configuration properties for your custom fields. Adding one is as simple as declaring the property in your custom class, then to use it, specify a configuration option with the same name as your property and the base field class will make sure it is set.

##### Example - Adding Properties
In your custom class simple declare an instance variable.
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

Then when using the field, you may simply add a configuration option with the same name.
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
It is fairly common for fields to have a label, so the `SiteOrigin_Widget_Field_Base` class includes a default label rendering function `render_field_label`. There are two ways to customise the label rendering. You can override `render_field_label` and do your own rendering, or you can override the `get_label_classes` function to return CSS classes to affect the styling of the existing label. The second method makes it easier for subclasses to customize the labels. You will need to ensure that your stylesheet containing the custom label CSS class is enqueued, for example in the field's `enqueue_scripts` method.

##### Example - Overriding `render_field_label`
```php
protected function render_field_label( $value, $instance ) {
	?>
	<h1>My custom label rendering</h1>
	<?php
}
```

##### Example - Adding Label CSS Classes
```php
protected function get_label_classes( $value, $instance ) {
	$label_classes = parent::get_label_classes( $value, $instance );
	$label_classes[] = 'additional-CSS-class';
	return $label_classes;
}
```

#### Rendering the Description
Similarly to the field label, the `SiteOrigin_Widget_Field_Base` class includes a default description rendering function `render_field_description`. It's default rendering may be customized in the same way as labels.

#### Render Before and After Field
The `SiteOrigin_Widget_Field_Base` class has two additional rendering methods, `render_before_field` and `render_after_field` which are called before and after the main rendering method. These serve to avoid duplication of commonly rendered items, such as a label above the field and a description below the field. You should override these if you wish to prevent rendering of the label before a field, or the description after a field, or if you want to render additional items.

##### Example - Overriding `render_before_field` and `render_after_field` Methods
Say you want to render the description after the label, but before the field.
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

#### The `sanitize_instance` Method
There are case where a field may affect values on the widget instance, other than it's own input. It then becomes necessary to perform additional sanitization on the widget instance. In such a case the `sanitize_instance` method may be overridden. It receives the widget instance and must return it.

#### The `get_related_instance_keys` Method
When the widget is saved, the Widgets Bundle removes every instance key that isn't a declared form field. If your field saves a value under its own sibling key, for example `{field name}_unit`, override `get_related_instance_keys` to return an array of those key names so the value is kept. The Widgets Bundle doesn't sanitize these keys, so sanitize them in `sanitize_instance`. The media field does both for its fallback URL key.

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
Override `get_default_options` to return an array of default values for your field's properties. Options passed in the form array override these defaults. Override `initialize` to run code after the options are set.

#### Scripts and Styles
Override the `enqueue_scripts` method to enqueue the field's JavaScript and CSS. The widget calls it for each field while it renders the form. To set up the field in JavaScript, listen for the `sowsetupformfield` event on the field's wrapper, which has the class `siteorigin-widget-field-type-{field type}`.

```javascript
jQuery( document ).on( 'sowsetupformfield', '.siteorigin-widget-field-type-better-text', function() {
	var $field = jQuery( this );
	// Set up the field here.
} );
```

#### JavaScript Variables
Occasionally it is necessary for a field to set a variable to be used in the widget form. For such cases, override the `get_javascript_variables` function. This will be called by the containing widget while it is rendering it's form and it will pass all field javascript variables to the browser, where they will be accessible in the global object `sow_field_javascript_variables`, keyed by widget class and then field name: `sow_field_javascript_variables[ widgetClass ][ fieldName ]`.


### Using a Custom Field
You can use your custom field in a widget, just like any other field.

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
