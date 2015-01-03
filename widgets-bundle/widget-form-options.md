# Widget form options

This is where you'll find a lot of the convenience of using the SiteOrigin Widgets Bundle as a framework for creating your own widgets. The widget form options are a way for you to define the configuration options you'd like to allow for your widget users. The more form options you provide, the more customizable your widget becomes.

## Form option field descriptors

Each value in the form options array is a form field descriptor, which is an associative array describing the form field to be rendered by the `SiteOrigin_Widget` base class, in order to capture configuration values for a widget instance. Each form field descriptor must at least have a type, however a few of the types won't be useful without additional configuration values. Optional base form field descriptor values are listed below:

- label: `string` Render a label for the field with the given value.
- default: `mixed` The field will be prepopulated with this default value.
- description: `string` Render small italic text below the field to desribe the field's purpose.
- optional: `bool` Append '(Optional)' to this field's label as a small green supertext.
- state_name: `string` Transforms into a class name, which, together with some JQuery, allows groups of elements to easily be set as visible or invisible simultaneously.
- hidden: `bool` Whether this field starts out visible or invisible.

In addition to these, some fields have their own specific configuration values, which are listed in the respective sections below.

## Form field types

### text
Renders a text input field.

#### Example
Form options input:
```
$form_options = array(
	'some_text' => array(
		'type' => 'text',
		'label' => __('Some text goes here', 'widget-form-fields-text-domain'),
		'default' => 'Some default text.'
	)
);
```
Result:

![Widget Form Text Input](./images/form-field-type-text.png)

---

### color
Renders a color input field and color picker.

#### Example
Form options input:
```
$form_options = array(
	'some_color' => array(
		'type' => 'color',
		'label' => __( 'Choose a color', 'widget-form-fields-text-domain' ),
		'default' => '#bada55'
	)
);
```
Result:

![Widget Form Color Picker](./images/form-field-type-color.png)

---

### number
Renders a text input field for entering a number. This is the same as the text type field, except that the input is cast as a `float`.

#### Example
Form options input:
```
$form_options = array(
	'some_number' => array(
		'type' => 'number',
		'label' => __( 'Enter a number', 'widget-form-fields-text-domain' ),
		'default' => '12654'
	)
);
```
Result:

![Widget Form Number Input](./images/form-field-type-number.png)

---

### textarea
Renders a textarea field.

#### Additional options
- allow_html_formatting: `bool` Allows a few specific html tags to be passed through sanitization to the widget instance to influence display of the text. These are `<a>`, `<br>`, `<strong>`, and `<em>`.
- rows: `int` The number of visible rows in the textarea.

#### Example
Form options input:
```
$form_options = array(
	'some_long_message' => array(
		'type' => 'textarea',
		'label' => __( 'Type a message', 'widget-form-fields-text-domain' ),
		'default' => 'An example of a long message.</br>It is even possible to add a few html tags.</br><a href="siteorigin.com" target="_blank"">Links!</a></br><strong>Strong</strong> and <em>emphasized</em> text.',
		'allow_html_formatting' => true,
		'rows' => 10
	)
);
```
Result:

![Widget Form Text Area](./images/form-field-type-textarea.png)

---

### slider
Renders a number slider field to allow the choice of a number in a range.

#### Additional options
- min: `int` The minimum value of the allowed range.
- max: `int` The maximum value of the allowed range.
- integer: `bool`

#### Example
Form options input:
```
$form_options = array(
	'some_number_in_a_range' => array(
		'type' => 'slider',
		'label' => __( 'Choose a number', 'widget-form-fields-text-domain' ),
		'default' => 24,
		'min' => 2,
		'max' => 37,
		'integer' => true
	)
);
```
Result:

![Widget Form Slider](./images/form-field-type-slider.png)

---

### select
Renders a dropdown select field. This field is better for a long list of predefined values. For a short list the radio input field is a better choice.

#### Additional options
- prompt: `string` If present, it is included as a disabled (not selectable) value at the top of the list of options. If there is no default value, it is selected by default. You might even want to leave the label value blank when you use this.
- options `array` The list of options which may be selected.

#### Example 1 - default value without prompt
Form options input:
```
$form_options = array(
	'some_selection' => array(
		'type' => 'select',
		'label' => __( 'Choose a thing from a long list of things', 'widget-form-fields-text-domain' ),
		'default' => 'the_other_thing',
		'options' => array(
			'this_thing' => __( 'This thing', 'widget-form-fields-text-domain' ),
			'that_thing' => __( 'That thing', 'widget-form-fields-text-domain' ),
			'the_other_thing' => __( 'The other thing', 'widget-form-fields-text-domain' ),
		)
	)
);
```
Result:

![Widget Form Select 1](./images/form-field-type-select-1.png)

#### Example 2 - prompt without default value
Form options input:
```
$form_options = array(
	'another_selection' => array(
		'type' => 'select',
		'prompt' => __( 'Choose a thing from a long list of things', 'widget-form-fields-text-domain' ),
		'options' => array(
			'this_thing' => __( 'This thing', 'widget-form-fields-text-domain' ),
			'that_thing' => __( 'That thing', 'widget-form-fields-text-domain' ),
			'the_other_thing' => __( 'The other thing', 'widget-form-fields-text-domain' ),
		)
	)
);
```
Result:

![Widget Form Select](./images/form-field-type-select-2.png)

---

### checkbox
Renders a checkbox field.

#### Example
Form options input:
```
$form_options = array(
	'some_boolean' => array(
		'type' => 'checkbox',
		'label' => __( 'Allow this thing?', 'widget-form-fields-text-domain' ),
		'default' => true
	)
);
```
Result:

![Widget Form Checkbox](./images/form-field-type-checkbox.png)

---

### radio
Renders a radio input field. This field is better for a short list of predefined values. For a long list the dropdown select field is a better choice.

#### Additional options
- options `array` The list of options which may be selected.

#### Example
Form options input:
```
$form_options = array(
	'radio_selection' => array(
		'type' => 'radio',
		'label' => __( 'Choose a thing from a short list of things', 'widget-form-fields-text-domain' ),
		'default' => 'that_thing',
		'options' => array(
			'this_thing' => __( 'This thing', 'widget-form-fields-text-domain' ),
			'that_thing' => __( 'That thing', 'widget-form-fields-text-domain' ),
			'the_other_thing' => __( 'The other thing', 'widget-form-fields-text-domain' )
		)
	)
);
```
Result:

![Widget Form Radio Input](./images/form-field-type-radio.png)

---

### media
Renders a media selector field. This fields requires at least WordPress 3.5.

#### Additional options
- choose: `string` A label for the title of the media selector dialog.
- update: `string` A label for the confirmation button of the media selector dialog.
- library: `string` Sets the media library which to browse and from which media can be selected. Allowed values are `'image'`, `'audio'`, `'video'`, and `'file'`. The default is `'file'`.

#### Example
Form options input:
```
$form_options = array(
	'some_media' => array(
		'type' => 'media',
		'label' => __( 'Choose a media thing', 'widget-form-fields-text-domain' ),
		'choose' => __( 'Choose image', 'widget-form-fields-text-domain' ),
		'update' => __( 'Set image', 'widget-form-fields-text-domain' ),
		'library' => 'image'
	)
);
```
Result:

![Widget Form Media Selector](./images/form-field-type-media.png)

---

### posts
Renders a post selector field. This can be used to build custom queries with which to select posts from your database. The field displays a small red indicator which shows the number of posts currently being selected. By default, all posts are selected.

#### Example
Form options input:
```
$form_options = array(
	'some_posts' => array(
		'type' => 'posts',
		'label' => __('Some posts query', 'siteorigin-widgets'),
	)
);
```
Result:

![Widget Form Posts Selector](./images/form-field-type-posts.png)

---

### icon

---

### section

---

### repeater

---

### widget

---