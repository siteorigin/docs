# Form Fields

Form fields are where the Widgets Bundle saves you the most work. You declare the fields your widget's users can set, and the Widgets Bundle builds the form, saves the values and passes them to your template.

## Field Descriptors

Your widget's `get_widget_form()` method returns the form as an array, as the Widgets Bundle's own widgets do. You can also pass the array to the `SiteOrigin_Widget` constructor, which stores it in `$form_options`. The Widgets Bundle calls `get_widget_form()` only when `$form_options` is empty.

Each item in the array is a field descriptor, an associative array that describes one field. Every descriptor needs a `type`, and some types need more options to be useful. Every field type accepts these options:

- `label` (`string`): the field's label.
- `default` (`mixed`): the field's starting value.
- `description` (`string`): small italic text under the field that explains it.
- `optional` (`bool`): adds a small green "(Optional)" after the label.
- `required` (`bool|string`): adds `*` after the label, and warns the user who saves the widget with the field empty. A string also shows as a message under the field.
- `sanitize` (`string|callable`): extra sanitization for non-empty input. The built-in values are `email`, `url`, `text`, which removes HTML for users without the `unfiltered_html` capability, and `number`, which casts the value to an integer. A PHP callable receives the value and, if it accepts a second argument, the field's previous value. Any other string runs the `siteorigin_widgets_sanitize_field_{$sanitize}` filter. [Input Sanitization](./input-sanitization.md) has the details.
- `state_emitter`, `state_handler` and `state_handler_initial` (`array`): show, hide or change fields based on the values of other fields, as [Modifying Forms With State Emitters](./state-emitters.md) explains.

Each field type below lists its own options too. The SiteOrigin Widget Form Fields Demo plugin in our [so-dev-examples](https://github.com/siteorigin/so-dev-examples) repository shows every field type in a working form.

## Field Types

### Text (`text`)

A text input.

#### Options

- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.
- `input_type` (`string`): the input's HTML type. Every standard [HTML input type](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) works.
- `allow_html` (`bool`): whether the saved value keeps HTML. The default, `true`, sanitizes the value with `wp_kses_post()`, so escape the value when you output it. `false` uses `sanitize_text_field()`.
- `json` (`bool`): `true` sanitizes the value as JSON.
- `onclick` (`bool`): `true` sanitizes the value as the JavaScript of an `onclick` attribute.
- `width` (`int`): the input's width in pixels.

The `allow_html`, `json` and `onclick` options also apply to the textarea and autocomplete fields. The `width` option also applies to the color, number, measurement and autocomplete fields.

#### Example

```php
$form_options = array(
	'some_text' => array(
		'type' => 'text',
		'label' => __( 'Some text goes here', 'siteorigin-docs' ),
		'default' => 'Some default text.'
	)
);
```

![Widget Form Text Input](../images/form-field-type-text.png)

---

### Link (`link`)

An input for a URL, with a button that searches public posts, except attachments. Output the value with `sow_esc_url()`, which turns a selected post's ID into the post's URL. The `siteorigin_widgets_search_posts_results` and `siteorigin_widgets_search_posts_order_by` filters change the search, as [Link Form Field Filters](./link-form-field-filters.md) explains.

#### Options

- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.
- `post_types` (`array`): the post types to search.
- `allow_shortcode` (`bool`): `true` keeps a value that contains `[` as a shortcode, and doesn't escape it as a URL.

#### Example

```php
$form_options = array(
	'some_url' => array(
		'type' => 'link',
		'label' => __( 'Some URL goes here', 'siteorigin-docs' ),
		'default' => 'http://www.example.com'
	)
);
```

![Widget Form Link Input](../images/form-field-type-link.png)

---

### Color (`color`)

A color input with a color picker.

#### Options

- `placeholder` (`string`): text shown in the empty field.
- `alpha` (`bool`): `true` adds an opacity slider to the color picker, and the value can be an `rgba()` color.
- `palettes` (`array`): hex colors to show as swatches in the color picker. The `siteorigin_widget_color_palette` filter adds swatches to every color field.

#### Example

```php
$form_options = array(
	'some_color' => array(
		'type' => 'color',
		'label' => __( 'Choose a color', 'siteorigin-docs' ),
		'default' => '#bada55'
	)
);
```

![Widget Form Color Picker](../images/form-field-type-color.png)

---

### Number (`number`)

A number input. The field works like the text field and saves the value as a `float`.

#### Options

- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.
- `abs` (`bool`): `true` runs `abs()` on the value when the widget is saved, so the value is never negative.
- `min` (`float`): the lowest value allowed.
- `max` (`float`): the highest value allowed.
- `step` (`float`): the step size of the input.
- `unit` (`string`): a unit shown beside the input. The unit isn't saved.

#### Example

```php
$form_options = array(
	'some_number' => array(
		'type' => 'number',
		'label' => __( 'Enter a number', 'siteorigin-docs' ),
		'default' => '12654',
		'unit' => 'px',
	)
);
```

![Widget Form Number Input](../images/form-field-type-number.png)

---

### Measurement (`measurement`)

An input for a CSS [length](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Values_and_units#Numeric_values), with a list of units beside it.

#### Options

- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.
- `units` (`array`): the units in the list. The default is the list from `siteorigin_widgets_get_measurements_list()`.
- `default_unit` (`string`): the unit the field uses when it can't detect the value's unit, or when the unit is no longer in `units`. The default is `px`.

#### Example

```php
$form_options = array(
	'example_size' => array(
		'type' => 'measurement',
		'label' => __( 'Size', 'siteorigin-docs' ),
		'default' => '10px',
	)
);
```

![Widget Form Measurement](../images/form-field-type-measurement.png)

---

### Multi-Measurement (`multi-measurement`)

Several [length](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Values_and_units#Numeric_values) inputs in one field, for values such as margins, borders and padding.

#### Options

- `measurements` (`array`): the inputs. Each item is a label, or an array with these keys:
    - `label` (`string`): the input's label.
    - `classes` (`array`): CSS classes for the input.
    - `units` (`array`): the units in the input's list. The default units are `px`, `%`, `in`, `cm`, `mm`, `em`, `rem`, `pt`, `pc`, `ex`, `ch`, `vw`, `vh`, `vmin` and `vmax`.
- `separator` (`string`): the separator between the saved values. The default is a space.
- `autofill` (`bool`): `true` fills the other inputs when the user enters the first value. The default is `false`.

#### Example

```php
$useable_units = array( 'px', '%' );
$form_options = array(
	'padding' => array(
		'type' => 'multi-measurement',
		'autofill' => true,
		'default' => '5% 0px 25px 0px',
		'measurements' => array(
			'top' => array(
				'label' => __( 'Padding Top', 'siteorigin-docs' ),
				'units' => $useable_units,
			),
			'right' => array(
				'label' => __( 'Padding Right', 'siteorigin-docs' ),
				'units' => $useable_units,
			),
			'bottom' => array(
				'label' => __( 'Padding Bottom', 'siteorigin-docs' ),
				'units' => $useable_units,
			),
			'left' => array(
				'label' => __( 'Padding Left', 'siteorigin-docs' ),
				'units' => $useable_units,
			),
		),
	),
);
```

![Widget Form Multi Measurement](../images/form-field-type-multi-measurement.png)

---

### Multiple Media (`multiple_media`)

A button that opens the WordPress Media Library, where users select several files of the types set in `library`. Use the type `multiple_media`, with an underscore, because the field's JavaScript doesn't load for `multiple-media`.

#### Options

- `choose` (`string`): the title of the media dialog.
- `update` (`string`): the label of the dialog's confirm button.
- `library` (`string`): the media types users can select: `'image'`, `'audio'`, `'video'`, `'file'` or `'application'`, or a comma-separated list of them. `'all'` allows every type. The default is `'image'`.
- `title` (`bool`): whether to show each item's title. The default is `true`.
- `thumbnail_dimensions` (`array`): the size of each thumbnail in the widget form. The default is `array( 64, 64 )`.
- `repeater` (`array`): a repeater field that the field adds items to, as [Connecting a Multiple Media Field to a Repeater](./multiple-media-repeater.md) explains.

#### Example

```php
$form_options = array(
	'images' => array(
		'type' => 'multiple_media',
		'label' => __( 'Multiple Media', 'siteorigin-docs' ),
		'library' => 'image',
		'thumbnail_dimensions' => array( 64, 64 ),
		'title' => true,
	),
);
```

![Widget Form Multi Media](../images/form-field-type-multiple-media.jpg)

---

### Textarea (`textarea`)

A textarea.

#### Options

- `rows` (`int`): the number of visible rows.
- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.

#### Example

```php
$form_options = array(
	'some_long_message' => array(
		'type' => 'textarea',
		'label' => __( 'Type a message', 'siteorigin-docs' ),
		'default' => 'An example of a long message.<br>It is even possible to add a few html tags.<br><a href="https://siteorigin.com" target="_blank">Links!</a><br><strong>Strong</strong> and <em>emphasized</em> text.',
		'rows' => 10
	)
);
```

![Widget Form Text Area](../images/form-field-type-textarea.png)

---

### TinyMCE (`tinymce`)

A TinyMCE editor.

#### Options

- `rows` (`int`): the number of visible rows.
- `default_editor` (`string`): the editor that shows first, `'tinymce'` (or `'tmce'`) for the visual editor or `'html'` for the Quicktags HTML editor. The default is `'tinymce'`.
- `media_buttons` (`bool`): whether to show the **Add Media** button. The default is `true`.
- `editor_height` (`int`): the editor's starting height. The field ignores `rows` when this option is set.
- `wpautop` (`bool`): whether `wpautop()` adds paragraphs when the value is saved from the visual editor. The default is `true`.
- `mce_buttons`, `mce_buttons_2`, `mce_buttons_3` and `mce_buttons_4` (`array`): the buttons in each row of the visual editor.
- `quicktags_buttons` (`array`): the buttons of the Quicktags HTML editor.
- `mce_plugins` (`array`): the TinyMCE plugins to load.
- `mce_external_plugins` (`array`): external TinyMCE plugins to load, as plugin name => script URL.
- `button_filters` (`array`): callbacks that filter the editor buttons. Each callback must be a method of your widget, in the form `array( $this, 'method_name' )`. The visual editor has up to four rows of buttons and the Quicktags editor has one, and each key filters one row:
    - `mce_buttons`: the first row.
    - `mce_buttons_2`: the second row.
    - `mce_buttons_3`: the third row.
    - `mce_buttons_4`: the fourth row.
    - `quicktags_settings`: the Quicktags settings.

#### Example

```php
$form_options = array(
	'some_tinymce_editor' => array(
		'type' => 'tinymce',
		'label' => __( 'Visually edit, richly.', 'siteorigin-docs' ),
		'default' => 'An example of a long message.<br>It is even possible to add a few html tags.<br><a href="https://siteorigin.com" target="_blank">Links!</a>',
		'rows' => 10,
		'default_editor' => 'html',
		'button_filters' => array(
			'mce_buttons' => array( $this, 'filter_mce_buttons' ),
			'mce_buttons_2' => array( $this, 'filter_mce_buttons_2' ),
			'mce_buttons_3' => array( $this, 'filter_mce_buttons_3' ),
			'mce_buttons_4' => array( $this, 'filter_mce_buttons_4' ),
			'quicktags_settings' => array( $this, 'filter_quicktags_settings' ),
		),
	)
);
```

![Widget Form Text Area](../images/form-field-type-tinymce.png)

---

### Slider (`slider`)

A slider for choosing a number in a range.

#### Options

- `min` (`float`): the lowest value. The default is `0`.
- `max` (`float`): the highest value. The default is `100`.
- `step` (`float`): the step size. The default is `1`.

#### Example

```php
$form_options = array(
	'some_number_in_a_range' => array(
		'type' => 'slider',
		'label' => __( 'Choose a number', 'siteorigin-docs' ),
		'default' => 24,
		'min' => 2,
		'max' => 37,
		'step' => 0.5,
	)
);
```

![Widget Form Slider](../images/form-field-type-slider.png)

---

### Order (`order`)

A list of options that users drag into order. [Order Field](./order-field.md) shows how to use the saved order.

#### Options

- `options` (`array`): the options to order.
- `default` (`array`): the keys of `options` in their starting order. Without a default, the field uses the order of `options`.

#### Example

```php
$form_options = array(
	'ordering' => array(
		'type' => 'order',
		'label' => __( 'Element Order', 'siteorigin-docs' ),
		'options' => array(
			'section' => __( 'Section', 'siteorigin-docs' ),
			'divider' => __( 'Content', 'siteorigin-docs' ),
			'other section' => __( 'Other Section', 'siteorigin-docs' ),
		),
		'default' => array( 'section', 'divider', 'other section' ),
	),
);
```

![Widget Form Ordering](../images/form-field-type-order.png)

---

### Select (`select`)

A dropdown. Use it for a long list of values, and the radio field for a short list.

#### Options

- `prompt` (`string`): a disabled first option. If the field has no default, the prompt is selected, and you can leave the label empty.
- `options` (`array`): the options.
- `multiple` (`bool`): `true` lets users select several options.
- `select2` (`bool`): `true` turns the field into a [Select2](https://select2.org) field.

The field ignores `prompt` when `multiple` is `true`.

#### Example: A Default Value Without a Prompt

```php
$form_options = array(
	'some_selection' => array(
		'type' => 'select',
		'label' => __( 'Choose a thing from a long list of things', 'siteorigin-docs' ),
		'default' => 'the_other_thing',
		'options' => array(
			'this_thing' => __( 'This thing', 'siteorigin-docs' ),
			'that_thing' => __( 'That thing', 'siteorigin-docs' ),
			'the_other_thing' => __( 'The other thing', 'siteorigin-docs' ),
		)
	)
);
```

![Widget Form Select 1](../images/form-field-type-select-1.png)

#### Example: A Prompt Without a Default Value

```php
$form_options = array(
	'another_selection' => array(
		'type' => 'select',
		'prompt' => __( 'Choose a thing from a long list of things', 'siteorigin-docs' ),
		'options' => array(
			'this_thing' => __( 'This thing', 'siteorigin-docs' ),
			'that_thing' => __( 'That thing', 'siteorigin-docs' ),
			'the_other_thing' => __( 'The other thing', 'siteorigin-docs' ),
		)
	)
);
```

![Widget Form Select](../images/form-field-type-select-2.png)

#### Example: Multiple Select

```php
$form_options = array(
	'another_selection' => array(
		'type' => 'select',
		'label' => __( 'Choose a thing from a long list of things', 'siteorigin-docs' ),
		'multiple' => true,
		'default' => 'the_other_thing',
		'options' => array(
			'this_thing' => __( 'This thing', 'siteorigin-docs' ),
			'that_thing' => __( 'That thing', 'siteorigin-docs' ),
			'the_other_thing' => __( 'The other thing', 'siteorigin-docs' ),
		)
	)
);
```

![Widget Form Multiple Select](../images/form-field-type-select-3.png)

---

### Checkbox (`checkbox`)

A checkbox.

#### Example

```php
$form_options = array(
	'some_boolean' => array(
		'type' => 'checkbox',
		'label' => __( 'Allow this thing?', 'siteorigin-docs' ),
		'default' => true
	)
);
```

![Widget Form Checkbox](../images/form-field-type-checkbox.png)

---

### Checkboxes (`checkboxes`)

A set of checkboxes.

#### Options

- `options` (`array`): the options.

#### Example

```php
$form_options = array(
	'potential_options' => array(
		'type' => 'checkboxes',
		'label' => __( 'Allow this thing?', 'siteorigin-docs' ),
		'options' => array(
			'option' =>  __( 'value', 'siteorigin-docs' ),
			'other option' =>  __( 'other value', 'siteorigin-docs' ),
			'another additional option' => __( 'Another possible value', 'siteorigin-docs' )
		),
	)
);
```

![Widget Form series of Checkboxes](../images/form-field-type-checkboxes.png)

---

### Radio (`radio`)

A set of radio buttons. Use it for a short list of values, and the select field for a long list.

#### Options

- `options` (`array`): the options.

#### Example

```php
$form_options = array(
	'radio_selection' => array(
		'type' => 'radio',
		'label' => __( 'Choose a thing from a short list of things', 'siteorigin-docs' ),
		'default' => 'that_thing',
		'options' => array(
			'this_thing' => __( 'This thing', 'siteorigin-docs' ),
			'that_thing' => __( 'That thing', 'siteorigin-docs' ),
			'the_other_thing' => __( 'The other thing', 'siteorigin-docs' )
		)
	)
);
```

![Widget Form Radio Input](../images/form-field-type-radio.png)

---

### Media (`media`)

A button that opens the WordPress Media Library, where users select a file of the types set in `library`.

#### Options

- `choose` (`string`): the title of the media dialog.
- `update` (`string`): the label of the dialog's confirm button.
- `library` (`string`): the media types users can select: `'image'`, `'audio'`, `'video'`, `'file'` or `'application'`, or a comma-separated list of them. `'all'` allows every type. The default is `'image'`.
- `fallback` (`bool`): `true` adds a field for a fallback URL, which the widget uses if the selected file isn't available. The fallback URL is saved under `{field name}_fallback`, such as `some_media_fallback`.
- `image_search` (`string`): the label of the **Image Search** button. The button shows only when `library` is `'image'` and the user can upload files.

#### Example

```php
$form_options = array(
	'some_media' => array(
		'type' => 'media',
		'label' => __( 'Choose a media thing', 'siteorigin-docs' ),
		'choose' => __( 'Choose image', 'siteorigin-docs' ),
		'update' => __( 'Set image', 'siteorigin-docs' ),
		'library' => 'image',
		'fallback' => true
	)
);
```

![Widget Form Media Selector](../images/form-field-type-media.png)

---

### Image Size (`image-size`)

A dropdown of the site's [image sizes](https://developer.wordpress.org/reference/functions/add_image_size/), which users pair with a media field to control the image's size. [Image Size Field](./image-sizes-field.md) shows how to output the image.

#### Options

- `custom_size` (`bool`): `true` lets users enter their own width and height. The default is `false`.
- `custom_size_enforce` (`bool`): `true` adds an **Enforce Dimensions** checkbox to a custom size, saved as `{field name}_enforce`.
- `sizes` (`array`): the sizes to list, as size name => label, in place of every registered size.

#### Example

```php
$form_options = array(
	'size' => array(
		'type' => 'image-size',
		'label' => __( 'Image size', 'siteorigin-docs' ),
	)
);
```

![Widget from Image Sizes](../images/form-field-type-image-sizes.png)

The sizes users can choose:

![Possible Image Size values](../images/form-field-type-image-sizes-example.png)

#### Custom Image Sizes

With `custom_size`, users can enter their own size. The field saves the width and height under its name with `_width` and `_height` added, such as `size_width` and `size_height`. When the field's value is `custom_size`, pass the width and height as an array:

```php
<?php
// Detect custom image size and override size.
if (
	$instance['size'] == 'custom_size' &&
	! empty( $instance['size_width'] ) &&
	! empty( $instance['size_height'] )
) {
	$instance['size'] = array(
		(int) $instance['size_width'],
		(int) $instance['size_height'],
	);
}

$src = siteorigin_widgets_get_attachment_image_src(
	$instance['image'], // Set elsewhere
	$instance['size']
);
```

---

### Posts (`posts`)

A post selector, where users build a query that selects posts. A small red badge shows how many posts the query selects, and the query selects every post of the `post` type until the user changes it. [Post Selector](./post-selector.md) shows how to use the query.

#### Options

- `show_count` (`bool`): whether to show the number of selected posts in the field's title. The default is `true`.
- `post_types` (`array`): the post types in the **Post Type** list.
- `posts_limit` (`bool`): `true` adds a **Maximum Posts to Output** setting.

#### Example

```php
$form_options = array(
	'some_posts' => array(
		'type' => 'posts',
		'show_count' => true,
		'label' => __( 'Some posts query', 'siteorigin-docs' ),
	)
);
```

![Widget Form Posts Selector](../images/form-field-type-posts.png)

---

### Section (`section`)

A group of related fields that users can collapse, which keeps a large form manageable.

#### Options

- `hide` (`bool`): `true` starts the section collapsed.
- `fields` (`array`): the fields in the section, of any type, including repeaters and sections.
- `tab` (`bool`): `true` shows the section as a tab of a tabs field, described under Tabs below.

#### Example

```php
$form_options = array(
	'a_section' => array(
		'type' => 'section',
		'label' => __( 'A section containing related fields.' , 'siteorigin-docs' ),
		'hide' => true,
		'fields' => array(
			'grouped_text' => array(
				'type' => 'text',
				'label' => __( 'A grouped text field', 'siteorigin-docs' )
			),
			'grouped_checkbox' => array(
				'type' => 'checkbox',
				'label' => __( 'A grouped checkbox', 'siteorigin-docs' )
			)
		)
	)
);
```

![Widget Form Section](../images/form-field-type-section.png)

---

### Tabs (`tabs`)

A row of tabs, where each tab shows one section. The field works only with sections, and on mobile devices the form shows the sections without tabs. The field needs Widgets Bundle 1.50.1 or later, and older versions show the sections as normal.

#### Options

- `tabs` (`array`): the sections to show as tabs, as section ID => tab label. The tab label can differ from the section's label.

```php
add_filter( 'siteorigin_widgets_form_options_sow-editor', function( $form_options ) {
	if ( empty( $form_options ) ) {
		return $form_options;
	}

	$form_options['tabs'] = array(
		'type' => 'tabs',
		'tabs' => array(
			'example_section' => __( 'Example Section', 'siteorigin-docs' ),
			'another_example' => __( 'Second Example', 'siteorigin-docs' ),
		),
	);

	$form_options['example_section'] = array(
		'type' => 'section',
		'label' => __( 'Example Section' , 'siteorigin-docs' ),
		'tab' => true,
		'hide' => true,
		'fields' => array(
			'test' => array(
				'type' => 'html',
				'markup' => __( 'First tab', 'siteorigin-docs' ),
			),
		),
	);


	$form_options['another_example'] = array(
		'type' => 'section',
		'label' => __( 'The Tab label defined above will be output instead of this' , 'siteorigin-docs' ),
		'tab' => true,
		'hide' => true,
		'fields' => array(
			'test' => array(
				'type' => 'html',
				'markup' => __( 'Second tab', 'siteorigin-docs' ),
			),
		),
	);


	return $form_options;
} );
```

![Tabs Form Field](../images/form-field-tabs.png)

---

### Repeater (`repeater`)

A set of fields that users can add as many times as they need. [Repeaters and Sections](./repeaters-and-sections.md) shows how to use the saved items.

#### Options

- `item_name` (`string`): the label of each item.
- `item_label` (`array`): how the repeater reads each item's label from a field in the item, with these keys:
    - `selector` (`string`): a jQuery selector for the element that holds the label.
    - `update_event` (`string`): the JavaScript event that updates the label.
    - `value_method` (`string`): the jQuery method that reads the label from the element.
- `fields` (`array`): the fields in each item, of any type, including repeaters and sections.
- `scroll_count` (`int`): the number of items to show before the repeater scrolls.
- `readonly` (`bool`): `true` stops users from adding and removing items.
- `max_items` (`int`): the most items the repeater can hold.

#### Example

```php
$form_options = array(
	'a_repeater' => array(
		'type' => 'repeater',
		'label' => __( 'A repeating repeater.' , 'siteorigin-docs' ),
		'item_name'  => __( 'Repeater item', 'siteorigin-docs' ),
		'item_label' => array(
			'selector'     => "[id*='repeat_text']",
			'update_event' => 'change',
			'value_method' => 'val'
		),
		'fields' => array(
			'repeat_text' => array(
				'type' => 'text',
				'label' => __( 'A text field in a repeater item.', 'siteorigin-docs' )
			),
			'repeat_checkbox' => array(
				'type' => 'checkbox',
				'label' => __( 'A checkbox in a repeater item.', 'siteorigin-docs' )
			)
		)
	)
);
```

![Widget Form Repeater 1](../images/form-field-type-repeater-1.png)

A repeater with two items, the first collapsed and the second expanded:

![Widget Form Repeater 3](../images/form-field-type-repeater-3.png)

---

### Widget (`widget`)

The full form of another widget, as [Child Widgets](./child-widgets.md) explains.

#### Options

- `class` (`string`): the class name of the widget to include.
- `hide` (`bool`): `true` starts the widget's form collapsed.
- `form_filter` (`callable`): a callback that receives the child widget's form options and returns the changed array.
- `collapsible` (`bool`): whether the child widget's form sits in a collapsible section. The default is `true`.

#### Example

```php
$form_options = array(
	'some_widget' => array(
		'type' => 'widget',
		'label' => __( 'Button Widget', 'siteorigin-docs' ),
		'class' => 'SiteOrigin_Widget_Button_Widget',
		'hide' => true
	)
);
```

![Widget Form Widget Field](../images/form-field-type-widget.png)

---

### Builder (`builder`)

A full [Page Builder](https://wordpress.org/plugins/siteorigin-panels/) layout. The field needs Page Builder, and [Builder Field](./builder-field.md) shows how to output the layout.

#### Options

- `builder_type` (`string`): the builder's type, output as its `data-type` attribute. The default is `sow-builder-field`.

#### Example

```php
$form_options = array(
	'page_builder' => array(
		'type' => 'builder',
		'label' => __( 'Page Builder', 'siteorigin-docs'),
	)
);
```

![Widget Form Builder field](../images/form-field-type-builder.png)

---

### Code (`code`)

A textarea with the [Behave.js](https://github.com/jakiestfu/Behave.js) code editing library. The field doesn't sanitize its value and ignores the `sanitize` option, so your widget must sanitize and escape the value itself.

#### Options

- `rows` (`int`): the number of visible rows.
- `placeholder` (`string`): text shown in the empty field.
- `readonly` (`bool`): `true` stops users from editing the field.

#### Example

```php
$form_options = array(
	'code_editor' => array(
		'type' => 'code',
		'label' => __( 'Code Editor', 'siteorigin-docs' ),
	)
);
```

![Widget Form Code Field](../images/form-field-type-code.png)

---

### Icon (`icon`)

An icon picker with the icon families from [Icons and Fonts](./icons-and-fonts.md). The `siteorigin_widgets_icon_families` filter adds your own families.

#### Options

- `icons_callback` (`callable`): a callback that returns the icon families to offer, in place of every family from the `siteorigin_widgets_icon_families` filter.
- `rows` (`int`): the number of rows of icons to show. The default is `3`.

#### Example

```php
$form_options = array(
	'some_icon' => array(
		'type' => 'icon',
		'label' => __( 'Select an icon', 'siteorigin-docs' ),
	)
);
```

![Widget Form Icon Selector](../images/form-field-type-icon.png)

---

### Font (`font`)

A font picker with the web-safe fonts (Arial, Courier New, Georgia, Helvetica Neue, Lucida Grande and Times New Roman) and the Google Fonts. The field uses the theme's font until the user picks one. The `siteorigin_widgets_font_families` filter adds your own fonts, as [Icons and Fonts](./icons-and-fonts.md) explains.

#### Example

```php
$form_options = array(
	'some_font' => array(
		'type' => 'font',
		'label' => __( 'Select a font', 'siteorigin-docs' ),
	)
);
```

The field before the user picks a font:

![Widget Form Font Selector](../images/form-field-type-font-1.png)

Picking a font:

![Widget Form Font Selector](../images/form-field-type-font-2.png)

---

### Presets (`presets`)

A list of ready-made settings for your widget, as [Presets](./presets.md) explains.

#### Options

- `options` (`array`): your presets.
- `default_preset` (`string`): the slug of a preset to select. Without it, the list starts with an empty option.

#### Example

```php
$form_options = array(
	'presets' => array(
		'type' => 'presets',
		'label' => __( 'Theme', 'siteorigin-docs' ),
		'default_preset' => 'test-2',
		'options' => array(
			'test' => array( // Preset 1
				'label' => 'Test 1',
				'values' => array(
					'test' => 'Test 1 example text',
				),
			),
			'test-2' => array(  // Preset 2
				'label' => 'Test 2',
				'values' => array(
					'test' => 'Test 1 example text',
					'color' => '#0f0',
				),
			),
		),
	),
);
```

![Widget Form Icon Selector](../images/form-field-type-preset.png)

---

### HTML (`html`)

HTML output in the form, for information that's clearer on its own than in a field description, such as an introduction to a section. The field needs Widgets Bundle 1.44.0 or later, and older versions show nothing.

#### Options

- `markup` (`string`): the HTML to output.

#### Example

This example adds two HTML fields to the end of the SiteOrigin Editor Widget's form: a box with inline styles, and the SiteOrigin logo.

```php
add_filter( 'siteorigin_widgets_form_options_sow-editor', function( $form_options ) {
	if ( empty( $form_options ) ) {
		return $form_options;
	}

	// This go anywhere in the `$form_options` array.
	$form_options['html_button_example'] = array(
		'type' => 'html',
		'markup' => '<span style="border: 1px solid #000; padding: 5px; margin: 21px; display: inline-block;">' . __( 'Box with inline styling', 'siteorigin-docs' ) . '</span>',
	);

	$form_options['siteorigin_logo'] = array(
		'type' => 'html',
		'label' => __( 'SiteOrigin Logo HTML Example' , 'siteorigin-docs' ),
		'markup' => '<img src="https://siteorigin.com/wp-content/themes/siteorigin-theme/images/logo/logo.svg" width="175" height="33">',
	);

	return $form_options;
} );

```

![HTML Form field](../images/form-field-html.png)

---

### Autocomplete (`autocomplete`)

A field that suggests posts or terms as the user types. The field saves a selected post's ID, or a term as `taxonomy:slug`, and separates several selections with commas.

#### Options

- `post_types` (`array`): the post types to suggest, when `source` is `posts`. The default is `post`.
- `source` (`string`): what the field suggests, `posts` or `terms`. The default is `posts`. The field sends its search to the `so_widgets_search_{source}` AJAX action.
- `multiple` (`bool`): whether users can select several items. The default is `true`.
- `ajax_data` (`array`): extra parameters for the AJAX request.

#### Example

```php
$form_options = array(
	'example' => array(
		'type' => 'autocomplete',
		'label' => __( 'Pages', 'siteorigin-docs'),
		'post_types' => array(
			'page'
		),
	),
);
```

![autocomplete Form field](../images/form-field-autocomplete.png)

---

### Toggle (`toggle`)

An on and off switch that shows its fields when it's on. The toggle saves its fields like a section, and saves the switch's state as `so_field_container_state`, which is `'open'` when the switch is on.

#### Options

- `fields` (`array`): the fields to show when the switch is on.
- `toggle_on` (`string`): the label of the on position. The default is "On".
- `toggle_off` (`string`): the label of the off position. The default is "Off".
- `hide` (`bool`): `true` starts the switch in the off position.

#### Example

```php
$form_options = array(
	'shadow' => array(
		'type' => 'toggle',
		'label' => __( 'Shadow', 'siteorigin-docs' ),
		'hide' => true,
		'fields' => array(
			'color' => array(
				'type' => 'color',
				'label' => __( 'Shadow color', 'siteorigin-docs' ),
			),
		),
	),
);
```

In your template, check the switch's state before you use the fields:

```php
if (
	! empty( $instance['shadow']['so_field_container_state'] ) &&
	$instance['shadow']['so_field_container_state'] === 'open'
) {
	// Use $instance['shadow']['color'] here.
}
```

---

### Image Radio (`image-radio`)

A set of radio buttons with an image for each option.

#### Options

- `options` (`array`): the options, as value => `array( 'image' => image URL, 'label' => label text )`.
- `layout` (`string`): `vertical` or `horizontal`. The default is `vertical`.

#### Example

```php
$form_options = array(
	'layout' => array(
		'type' => 'image-radio',
		'label' => __( 'Layout', 'siteorigin-docs' ),
		'default' => 'left',
		'layout' => 'horizontal',
		'options' => array(
			'left' => array(
				'label' => __( 'Image left', 'siteorigin-docs' ),
				'image' => plugin_dir_url( __FILE__ ) . 'images/left.svg',
			),
			'right' => array(
				'label' => __( 'Image right', 'siteorigin-docs' ),
				'image' => plugin_dir_url( __FILE__ ) . 'images/right.svg',
			),
		),
	),
);
```

---

### Other Field Types

The Widgets Bundle also has the `date-range` field, which the post selector uses, and the `image_shape` field, which the Image Widget's **Image Shape** setting uses. Set the date range field's `date_type` to `specific` or `relative`. Use the `image_shape` type, with an underscore, because the field's JavaScript doesn't load for `image-shape`.
