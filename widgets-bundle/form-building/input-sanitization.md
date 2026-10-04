# Input Sanitization

When a user saves a widget, `SiteOrigin_Widget::update()` passes each field's value to that field's `sanitize()` method. Each field type sanitizes its value in its own way, and filters add more sanitization. Most fields save an empty string or `null` as an empty string, and sanitize every other value as described below. Container fields, such as sections, repeaters and child widgets, save an empty value as an empty array.

The Widgets Bundle has its own version of the WordPress `esc_url_raw()` function, `sow_esc_url_raw()`. It also accepts the `skype:` and `steam:` URL protocols, which the `siteorigin_esc_url_protocols` filter changes, and our own `post:` protocol, which it turns into the URL of the post with that ID.

## Sanitization by Field Type

### Select and Radio Fields

If the value isn't one of the field's options, the field saves its default, or `false` if it has no default.

### Number Fields

A value that isn't numeric becomes `false`. A numeric value is limited by the field's `min` and `max` options, made positive if the `abs` option is set, and cast to a float. A `min` or `max` of `0` has no effect.

### Slider Fields

The value is cast to a float.

### Text and Textarea Fields

The value passes through `wp_kses_post()` and then `balanceTags()`, so users can enter some HTML tags but no JavaScript, and unclosed tags are fixed. If the field's `allow_html` option is `false`, the field uses `sanitize_text_field()` in place of `wp_kses_post()`. If the `json` option is set, the field sanitizes the value as JSON.

### Color Fields

The field adds a missing `#` to the start of the value, then checks that the value is a `#` followed by 3 or 6 hexadecimal characters. A value that doesn't match becomes `false`. If the field's `alpha` option is set, the field also accepts an `rgba(r,g,b,a)` value.

### Media Fields

The value is an attachment ID, so it passes through `intval()`. If the field has a fallback URL, the URL passes through `sow_esc_url_raw()`.

### Link Fields

The field trims the value, then checks whether it is `post:` followed by a post ID. Any other value is treated as a URL and passes through `sow_esc_url_raw()`. If the field's `allow_shortcode` option is set, a value that contains `[` passes through `esc_attr()` so that it can hold a shortcode.

### Checkbox Fields

Any non-empty value other than the string `'false'` becomes `true`, and every other value becomes `false`. An unchecked checkbox sends no value, so it saves an empty string.

### Widget Fields

Each field in the child widget's form is sanitized by its own field type, the same way as fields in a section.

### Repeater Fields

Each repeater item passes through `sanitize()`.

### Section Fields

The section passes through `sanitize()`.

### Unknown Field Types

A field with an unknown type shows an error message in the widget form, and its value isn't sanitized. Check that every field in your form uses a valid field type.

## Extra Sanitization

A field's `sanitize` option adds more sanitization, which runs only when the value isn't empty. The option accepts four built-in values:

- `url`: sanitizes the value as a URL with `sow_esc_url_raw()`.
- `email`: sanitizes the value as an email address with `sanitize_email()`.
- `text`: removes HTML with `sanitize_text_field()` when the user who saves the widget doesn't have the `unfiltered_html` capability.
- `number`: casts the value to an integer.

The `sanitize` option also accepts any PHP callable, which receives the value and returns the sanitized value. If the callable accepts a second argument, it also receives the field's previous value. PHP's own functions, such as `intval`, receive only the value unless they require two arguments.

### Example: Built-In Options

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

### Example: A Custom Option

Any other string in the `sanitize` option runs the `siteorigin_widgets_sanitize_field_{$sanitize}` filter, where `{$sanitize}` is the string. The filter receives the value. This example adds a `date` option:

```php
function my_widgets_sanitize_date( $date_to_sanitize ) {
	// Perform custom date sanitization here.
	$sanitized_date = sanitize_text_field( $date_to_sanitize );
	return $sanitized_date;
}
add_filter( 'siteorigin_widgets_sanitize_field_date', 'my_widgets_sanitize_date' );
```

Then set the field's `sanitize` option to `date`:

```php
'some_date' => array(
	'type' => 'text',
	'label' => __( 'Some date goes here', 'siteorigin-docs' ),
	'sanitize' => 'date',
),
```

### Example: A Callable

The `sanitize` option can also name a function, with no filter:

```php
function my_widgets_sanitize_date_field( $date_to_sanitize, $old_value = null ) {
	return sanitize_text_field( $date_to_sanitize );
}

$form_options = array(
	'some_date' => array(
		'type' => 'text',
		'label' => __( 'Some date goes here', 'siteorigin-docs' ),
		'sanitize' => 'my_widgets_sanitize_date_field',
	),
);
```

## Sanitizing the Whole Instance

Just before `update()` returns the sanitized instance, two more filters let other plugins sanitize it: `siteorigin_widgets_sanitize_instance` and `siteorigin_widgets_sanitize_instance_{$id_base}`, where `{$id_base}` is the widget's base ID. Both filters pass three arguments: the new instance, the form options and the widget object. Use `add_filter( 'siteorigin_widgets_sanitize_instance', 'my_callback', 10, 3 )` to receive all three.

## Undeclared Instance Keys

When a user saves a widget, the Widgets Bundle removes every instance key that isn't a form field. It keeps the keys that a field declares with its `get_related_instance_keys()` method, such as a media field's fallback URL, along with `_sow_form_id`, `_sow_form_timestamp` and `panels_info`. If your widget stores extra values, add a field for them to the form. Fields inside sections and repeaters follow the same rule.
