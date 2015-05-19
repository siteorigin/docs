# Sanitization
The `SiteOrigin_Widget` abstract base class sanitizes all widget instance input fields using it's `sanitize` function. The sanitization method varies for different field types and additional sanitization may be done using filters.

>Note: We have included a wrapper for the built-in WordPress `esc_url_raw()` function, named `sow_esc_url_raw()`. It performs the same function, but additionally allows the "skype:" URL protocol and our own "post:" protocol which we convert into a real URL using the specified post ID.

## Built-in sanitization for field types

### Select and radio fields
If the selected value is not present in the list of values originally presented to the front end, then the value reverts to the field's specified default. If there is no default specified, the value reverts to `false`.

### Number and slider fields
The input value must be a number, so the value is simply typecast to float.
 
### Textarea and text fields
The value is sanitized using two WordPress sanitization functions, namely `wp_kses_post()` followed by a forced `balanceTags()` call. This allows users to input some HTML tags, but not JavaScript and attempts to fix any mistakes made by users when inputting HTML tags.

### Color fields
The value is checked against a regular expression pattern which ensures the value starts with the '#' character followed by either 3 or 6 characters in the ranges 'A-F', 'a-f' and '0-9'. If it does not match the required pattern it is set to `false`.

### Media fields
The value should be an integer so is passed through the `intval()` function. Additionally, if the optional 'fallback' URL is specified, it is escaped using the built-in WordPress function `esc_url_raw()`.

### Link fields
The value is stripped of leading and trailing whitespace and then checked against a regular expression which ensures the value starts with 'post:' followed by at least one digit. If it does not match the required pattern it is assumed to be a URL and escaped using the built-in WordPress function `esc_url_raw()`. 

### Checkbox fields
If the value is any non-empty value it is set to true, otherwise it is set to false.

### Widget fields
The widget class is instantiated and it's update method is called which handles it's own sanitization.

### Repeater fields
The repeater items are iterated over and each item is passed into the `sanitize` function to be sanitized.

### Section fields
A section is passed into the `sanitize` function to be sanitized.

### Default sanitization
If the type of field is not recognized, the built-in WordPress function `sanitize_text_field()` is used to sanitize the field.

## Additional sanitization
Additionally, a 'sanitize' option may be set on form options to specify extra sanitization which should occur. There are two existing additional 'sanitize' options:
- url: Lets the field be sanitized as a URL using the `sow_esc_url_raw()` function.
- email: Lets the field be sanitized as an email address using the built-in WordPress `sanitize_email()` function

### Example - additional sanitization options
```php
$form_options = array(
	'some_url' => array(
		'type' => 'link',
		'label' => __('Some URL goes here', 'widget-form-fields-text-domain'),
		'sanitize' => 'url',
	),
	'some_email_address' => array(
	    'type' => 'text',
	    'label' => __( 'Some email address goes here', 'widget-form-fields-text-domain' ),
	    'sanitize' => 'email',
	),
);
```

If any other string is specified for the 'sanitize' option, it is assumed to be a custom sanitization and a filter is applied using a concatenation of 'siteorigin_widgets_sanitize_field_' and the specified string.

### Example - custom sanitization options
```php
function __construct() {
    $form_options = array(
        'some_date' => array(
            'type' => 'text',
            'label' => __( 'Some date goes here', 'widget-form-fields-text-domain' ),
            'sanitize' => 'date',
        ),
    );
    // Parent constructor is called with $form_options here.
}

add_filter( 'siteorigin_widgets_sanitize_field_date', array( $this, 'sanitize_date' ) );

function sanitize_date( $date_to_sanitize ) {
    // Perform custom date sanitization here.
    $sanitized_date = sanitize_text_field( $date_to_sanitize );
    return $sanitized_date;
}
```


Finally, just before the sanitized instance is returned, two more filters are applied to allow other plugins to perform their own sanitization. The filters are `'siteorigin_widgets_sanitize_instance'` and `'siteorigin_widgets_sanitize_instance_' . $this->id_base`, where `$this->id_base` is the base ID of the widget class which is performing the sanitization.
