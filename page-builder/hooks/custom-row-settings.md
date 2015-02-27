### Adding a custom option under the Row Styles

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

####Parameters:

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


### Adding the new option to the row element

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
