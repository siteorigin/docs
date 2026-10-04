# Sanitization

When a widget is saved, `SiteOrigin_Widget::update()` passes each field value to that field's `sanitize()` method. The sanitization method varies for different field types and additional sanitization may be done using filters.

>Note: We have included a wrapper for the built-in WordPress `esc_url_raw()` function, named `sow_esc_url_raw()`. It performs the same function, but additionally allows the "skype:" and "steam:" URL protocols (filterable with `siteorigin_esc_url_protocols`) and our own "post:" protocol which we convert into a real URL using the specified post ID.

## Built-in sanitization for field types

### Select and radio fields
If the selected value is not present in the list of values originally presented to the front end, then the value reverts to the field's specified default. If there is no default specified, the value reverts to `false`.

### Number fields
If the value isn't numeric, it is set to `false`. Otherwise it is limited by the field's `min` and `max` options, made positive if the `abs` option is set, and typecast to float.

### Slider fields
The value is typecast to float.

### Textarea and text fields
The value is sanitized using two WordPress sanitization functions, namely `wp_kses_post()` followed by a forced `balanceTags()` call. This allows users to input some HTML tags, but not JavaScript and attempts to fix any mistakes made by users when inputting HTML tags. If the field's `allow_html` option is `false`, `sanitize_text_field()` is used in place of `wp_kses_post()`. If the `json` option is set, the value is sanitized as JSON.

### Color fields
A missing '#' is added to the start of the value. The value is then checked against a regular expression pattern which ensures the value starts with the '#' character followed by either 3 or 6 characters in the ranges 'A-F', 'a-f' and '0-9'. If it does not match the required pattern it is set to `false`. If the field's `alpha` option is set, an `rgba(r,g,b,a)` value is also accepted.

### Media fields
The value should be an integer so is passed through the `intval()` function. Additionally, if the optional 'fallback' URL is specified, it is escaped using `sow_esc_url_raw()`.

### Link fields
The value is stripped of leading and trailing whitespace and then checked against a regular expression which ensures the value starts with 'post:' followed by at least one digit. If it does not match the required pattern it is assumed to be a URL and escaped using `sow_esc_url_raw()`. If the field's `allow_shortcode` option is set, a value containing `[` is escaped with `esc_attr()` so that it can hold a shortcode.

### Checkbox fields
If the value is any non-empty value other than the string `'false'`, it is set to true, otherwise it is set to false.

### Widget fields
Each field of the child widget's form is sanitized by its own field type, the same way section fields are.

### Repeater fields
The repeater items are iterated over and each item is passed into the `sanitize` function to be sanitized.

### Section fields
A section is passed into the `sanitize` function to be sanitized.

### Unknown field types
If the type of field is not recognized, the field displays an error message in the widget form and its value is not sanitized. Check that every field in your form uses a valid field type.

## Additional sanitization
Additionally, a 'sanitize' option may be set on form options to specify extra sanitization which should occur. Extra sanitization only runs when the value is not empty. There are four existing additional 'sanitize' options:
- url: Lets the field be sanitized as a URL using the `sow_esc_url_raw()` function.
- email: Lets the field be sanitized as an email address using the built-in WordPress `sanitize_email()` function.
- text: Removes HTML using `sanitize_text_field()` when the user saving the widget doesn't have the `unfiltered_html` capability.
- number: Typecasts the value to an integer.

The 'sanitize' option can also be any PHP callable. The callable receives the value, followed by the field's previous value, and returns the sanitized value.

### Example - additional sanitization options
```php
$form_options = array(
	'some_url' => array(
		'type' => 'link',
		'label' => __( 'Some URL goes here', 'siteorigin-docs' ),
		'sanitize' => 'url',
	),
	'some_email_address' => array(
		'type' => 'text',
		'label' => __( 'Some email address goes here', 'siteorigin-docs' ),
		'sanitize' => 'email',
	),
);
```

If any other string is specified for the 'sanitize' option, it is assumed to be a custom sanitization and a filter is applied using a concatenation of 'siteorigin_widgets_sanitize_field_' and the specified string. The filter receives one argument, the value.

### Example - custom sanitization options
```php
function my_widgets_sanitize_date( $date_to_sanitize ) {
	// Perform custom date sanitization here.
	$sanitized_date = sanitize_text_field( $date_to_sanitize );
	return $sanitized_date;
}
add_filter( 'siteorigin_widgets_sanitize_field_date', 'my_widgets_sanitize_date' );
```

Then set the 'sanitize' option to `date` in your widget's form.

```php
'some_date' => array(
	'type' => 'text',
	'label' => __( 'Some date goes here', 'siteorigin-docs' ),
	'sanitize' => 'date',
),
```

### Example - callable sanitization
In a widget class, the 'sanitize' option can point to one of the widget's own methods.

```php
class My_Date_Widget extends SiteOrigin_Widget {
	// The constructor is left out here.

	function get_widget_form() {
		return array(
			'some_date' => array(
				'type' => 'text',
				'label' => __( 'Some date goes here', 'siteorigin-docs' ),
				'sanitize' => array( $this, 'sanitize_date' ),
			),
		);
	}

	function sanitize_date( $date_to_sanitize ) {
		return sanitize_text_field( $date_to_sanitize );
	}
}
```


Finally, just before the sanitized instance is returned, two more filters are applied to allow other plugins to perform their own sanitization. The filters are `'siteorigin_widgets_sanitize_instance'` and `'siteorigin_widgets_sanitize_instance_' . $this->id_base`, where `$this->id_base` is the base ID of the widget class which is performing the sanitization. Both filters receive three arguments: the new instance, the form options and the widget object.

## Undeclared instance keys
When a widget is saved, the Widgets Bundle removes every instance key that isn't a declared form field. Keys that a field declares with its `get_related_instance_keys()` method, such as a media field's fallback URL, are kept, along with `_sow_form_id`, `_sow_form_timestamp` and `panels_info`. If your widget stores extra values, add a field for them to the form. Fields inside sections and repeaters follow the same rule.
