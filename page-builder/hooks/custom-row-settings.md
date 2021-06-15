### Adding a Custom Option Under the Row Styles

```php
function custom_row_style_fields($fields) {
  $fields['parallax'] = array(
  	'name'        => __('Parallax', 'siteorigin-panels'),
  	'type'        => 'checkbox',
  	'group'       => 'design',
  	'description' => __('If enabled, the background image will have a parallax effect.', 'siteorigin-panels'),
  	'priority'    => 8,
  );
  
  return $fields;
}

add_filter( 'siteorigin_panels_row_style_fields', 'custom_row_style_fields' );
```

#### Parameters:

**name**

The name of the custom option.

**type**

Can be set to ``checkbox``, ``text``, ``code``, ``measurement``, ``color``, ``image`` and ``select``.

**group**

Can be set to ``design``, ``layout`` and ``attributes``.

**description**

(Optional) A description of the custom option.

**priority**

The position in the group the option should appear. Priority set on default options:

Attribute Fields

* 5 - Row Class
* 6 - Cell Class
* 10 - CSS Styles

Layout Fields

* 5 - Bottom Margin
* 6 - Gutter
* 7 - Padding
* 10 - Row Layout

Design Fields

* 5 - Background Color
* 6 - Background Image
* 7 - Background Image Display
* 10 - Border Color

### Adding the New Option to the Row Element

```php
function custom_row_style_attributes( $attributes, $args ) {
	if( !empty( $args['parallax'] ) ) {
		array_push($attributes['class'], 'parallax');
	}
	
	return $attributes;
}

add_filter('siteorigin_panels_row_style_attributes', 'custom_row_style_attributes', 10, 2);
```

The code checks if parallax is set inside the ``$args`` array and adds a value of 'parallax' to ``$attributes['class']``.

``$args`` array is used to check which values have been set.
``$attributes`` array contains all the values that will display on the front-end.

To add a custom class, append the class name to ``$attributes['class']``.

To add a custom style (like text-align: center;), append the style to ``$attributes['style']``.

To add a new attribute, add both a new key and value to ``$attributes``. Example: ``$attributes['data-video-id'] = $args['video-id'];`` will add ``data-video-id="9"`` to the row element.

Note that the filter for ``siteorigin_panels_row_style_attributes`` needs to be called after Page Builder loads. In my project I added the filter inside a function which is triggered on ``plugins_loaded``.

### Processing Styles

Before rendering the context's styles, it's possible to access and alter the styles of the current context using filters. This allows you to migrate and alter data as needed. Changes aren't, however, stored until the user saves the page/widget. There are two filters that allow for this. `siteorigin_panels_general_current_styles`, which is a general catch-all filter, and `siteorigin_panels_general_current_styles_{type}`, `{type}` being the currently viewed type.

#### Parameters:

**current**

An array containing the current settings for the type.

**post_id**

The id of post being edited. This value will be empty if edited outside of Page Builder.

**type**

The current type. This parammater is not present if `siteorigin_panels_general_current_styles_{type}` is the used filter.

**args**

An array containing builder arguments.

### Altering the Default Number of Columns for Rows

It's possible to override the default number of row columns, and their weight (width), by using the `siteorigin_panels_default_row_columns` filter. This filter accepts a multidimensional array with each item being treated as a column. Each item must contain a weight item which is used to size the column. Weight is decimal based so 0.30 is equivalent to 30% of the row.

The following snippet will allow you to set the default

```php
add_filter( 'siteorigin_panels_default_row_columns', function( $default_columns ) {
	return array(
		array(
			'weight' => 0.333,
		),
		array(
			'weight' => 0.333,
		),
		array(
			'weight' => 0.333,
		),
	);
} );
```

### Override the Row Column Input

It's possible to adjust the Row Column input markup by using the `siteorigin_panels_row_column_count_input` filter. This will allow alter the column number present in the input field. It doesn't however allow you to alter the default row columns, that's done using `siteorigin_panels_default_row_columns` filter.

The following snippet will column the column field to 4.

```php
add_filter( 'siteorigin_panels_row_column_count_input', function( $input ) {
	return '<input type="number" min="1" max="12" name="cells" class="so-row-field" value="4" />';
} );
```
