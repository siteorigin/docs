# Adding custom fields
We have made the form fields, used by SiteOrigin widgets, extendible so that you can easily create your own custom fields. There are a few steps involved, but each of them is fairly simple. You can see the example code in the so-dev-examples repository <a href="https://github.com/siteorigin/so-dev-examples/tree/develop/extend-widgets-bundle/custom-fields/" target="_blank">here</a>.

## Field class names
The SiteOrigin Widgets Bundle supports PHP 5.2.0 and above, so we avoid the use of the namespaces feature which is only available from PHP 5.3.0 onwards. We "namespace" our classes by prefixing their names with some (hopefully unique) prefix. The full class name then follows the convention `$class_prefix . ucfirst($field_type)`. For example, the basic text field in the Widgets Bundle has the type `text` and it is prefixed by `SiteOrigin_Widget_Field_`, so the resulting class name is `SiteOrigin_Widget_Field_Text`. Note that the field type has a capitalised first letter.

## Adding custom field class prefixes
We encourage you to prefix your custom field class names to avoid conflicts with other class names. If you do this, the Widgets Bundle needs to know what your chosen prefix is, in order to autoload and instantiate your custom field classes. If you need to, you can add more than one prefix, but one is sufficient for the Widgets Bundle.

### Example - adding class prefixes
```php
function my_custom_fields_class_prefixes( $class_prefixes ) {
	$class_prefixes[] = 'My_Custom_Field_';
	return $class_prefixes;
}
add_filter( 'siteorigin_widgets_field_class_prefixes', 'my_custom_fields_class_prefixes' );
```

## Adding custom field class paths
It is necessary for the Widgets Bundle to know which directory you custom field class files are kept in for the purpose of autoloading. You can add your class paths to the autoloader by adding the `siteorigin_widgets_field_class_paths` filter.

### Example - adding class paths
```php
function my_custom_fields_class_paths( $class_paths ) {
	$class_paths[] = plugin_dir_path( __FILE__ ) . 'custom-fields/';
	return $class_paths;
}
add_filter( 'siteorigin_widgets_field_class_paths', 'my_custom_fields_class_paths' );
```
## Implementing a custom field
Implementing a custom field is as simple as extending one of the existing field classes and implementing or overriding at least the `render_field` and `sanitize_input` methods. There is much more that can be done, but this is all that is required to successfully render a custom field and save it's input.

### Filenames and class naming
For your field class to be loaded, you need to name your class according to the convention mentioned above. However the file itself must be named according to the convention `$field_type.class.php` and it must be placed in one of the class paths you added in the step above. For example, if you have a field type of `taxonomylist` with a custom class path of `my_custom_fields/` and a class prefix of `My_Custom_Field_`, you'd first create the file `my_custom_fields/taxonomylist.class.php` and then define the class `My_Custom_Field_Taxonomylist` inside it. 

### Inheriting from SiteOrigin_Widget_Field_Base
The `SiteOrigin_Widget_Field_Base` abstract class handles most of the work required for the widget form fields. It contains various properties and methods which are used to render the field for display in the front end and preparing input from the field for database persistence. When extending this class there are two abstract methods which must be implemented, namely, `render_field` and `sanitize_input`.

#### The `render_field` method
`render_field` should output the HTML required for your custom field's display in the front end. The method receives two arguments, `$value` and `$instance`. `$value` is the current value of the field for a specific instance of a widget form and should always be escaped just before output. `$instance` is the widget form instance containing all it's current values.

##### Example - `render_field` implementation
```php
protected function render_field( $value, $instance ) {
	?>
	<input type="text" id="<?php echo $this->element_id ?>" name="<?php echo $this->element_name ?>"
		   value="<?php echo esc_attr( $value ); ?>"/>
	<?php
}
```

#### The `sanitize_input` method
`sanitize_input` should ensure that the input received from the front end is in the desired format and any unwanted characters are removed. It receives one argument, `$value`, which is the raw current value of the field input in the front end. Typically this value is sanitized using the built-in WordPress sanitization and escaping functions.

##### Example - `sanitize_input` implementation
```php
protected function sanitize_field_input( $value ) {
	$sanitized_value = sanitize_text_field( $value );
	return $sanitized_value;
}
```

#### Adding properties
You may wish to have additional configuration properties for your custom fields. Adding one is as simple as declaring the property in your custom class, then to use it, specify a configuration option with the same name as your property and the base field class will make sure it is set.

##### Example - adding properties
In your custom class simple declare an instance variable.
```php
class My_Custom_Field_Better_Text extends SiteOrigin_Widget_Field_Base {
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
		'label' => __('A better text field.', 'my-custom-field-test-widget-text-domain'),
		'default' => 'Some better text.'
	),
),
```

#### Rendering the label
It is fairly common for fields to have a label, so the `SiteOrigin_Widget_Field_Base` class includes a default label rendering function `render_field_label`. There are two ways to customise the label rendering. You can override `render_field_label` and do your own rendering, or you can override the `get_label_classes` function to return CSS classes to affect the styling of the existing label. The second method makes it easier for subclasses to customize the labels. You will need to ensure that your stylesheet containing the custom label CSS class is enqueued elsewhere.

##### Example - overriding `render_field_label`
```php
protected function render_field_label() {
	?>
	<h1>My custom label rendering</h1>
	<?php
}
```

##### Example - adding label CSS classes
```php
protected function get_label_classes() {
	$label_classes = parent::get_label_classes();
	$label_classes[] = 'additional-CSS-class';
	return $label_classes;
}
```

#### Rendering the description
Similarly to the field label, the `SiteOrigin_Widget_Field_Base` class includes a default description rendering function `render_field_description`. It's default rendering may be customized in the same way as labels.

#### Render before and after field
The `SiteOrigin_Widget_Field_Base` class has two additional rendering methods, `render_before_field` and `render_after_field` which are called before and after the main rendering method. These serve to avoid duplication of commonly rendered items, such as a label above the field and a description below the field. You should override these if you wish to prevent rendering of the label before a field, or the description after a field, or if you want to render additional items.

##### Example - overriding `render_before_field` and `render_after_field` methods
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

#### The `sanitize_instance` method
There are case where a field may affect values on the widget instance, other than it's own input. It then becomes necessary to perform additional sanitization on the widget instance. In such a case the `sanitize_instance` method may be overridden. 

#### JavaScript variables
Occasionally it is necessary for a field to set a variable to be used in the front end. For such cases, override the `get_javascript_variables` function. This will be called by the containing widget while it is rendering it's form and it will pass all field javascript variables to the front end where they will be accessible as a global object called `sow_field_javascript_variables`.  


### Using a custom field
You can use your custom field in a widget, just like any other field.

```php
$form_options = array(
	'text' => array(
		'type' => 'better-text',
		'my_property' => 'This is my custom property value',
		'label' => __( 'A better text field.', 'my-custom-field-test-widget-text-domain' ),
		'description' => __( 'A description for my custom text field.' ),
		'default' => 'Some better text.'
	),
);
```
