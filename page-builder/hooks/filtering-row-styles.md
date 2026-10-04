# Filtering Custom Row Options

The `siteorigin_panels_row_style_fields` filter adds your own fields to the row settings in Page Builder. The filter also passes `$post_id` and `$args`, the builder arguments. Columns and widgets have their own filters, `siteorigin_panels_cell_style_fields` and `siteorigin_panels_widget_style_fields`, and `siteorigin_panels_general_style_fields` adds a field to rows, columns and widgets at once.

## Adding a Row Field

This example adds a **Parallax** checkbox to the **Design** group of the row settings:

```php
function custom_row_style_fields( $fields ) {
	$fields['parallax'] = array(
		'name'        => __( 'Parallax', 'so-widgets-test' ),
		'type'        => 'checkbox',
		'group'       => 'design',
		'description' => __( 'If enabled, the background image will have a parallax effect.', 'so-widgets-test' ),
		'priority'    => 8,
	);

	return $fields;
}

add_filter( 'siteorigin_panels_row_style_fields', 'custom_row_style_fields' );
```

Each field takes these keys:

- `name`: the field's label.
- `type`: the field type. The types are `checkbox`, `code`, `color`, `image`, `image_size`, `measurement`, `multi-select`, `number`, `radio`, `select`, `slider`, `text`, `textarea`, `toggle` and `url`.
- `group`: the settings group the field appears in, one of `attributes`, `layout`, `tablet_layout`, `mobile_layout` and `design`. The `tablet_layout` group appears only when **Use Tablet Layout** is enabled in the Page Builder settings. A field with no group, or with the `theme` group, appears in a **Theme** group.
- `description`: optional help text under the field.
- `priority`: the field's position in its group. A field with a lower number appears higher.

These are the priorities of Page Builder's own row fields, so you can place your field between them:

| Group | Priority and field |
|---|---|
| Attributes | 4 **Row ID**, 5 **Row Class**, 6 **Column Class**, 10 **CSS Declarations**, 11 **Mobile CSS Declarations** |
| Layout | 5 **Bottom Margin**, 6 **Gutter**, 7 **Padding**, 10 **Row Layout**, 15 **Collapse Behaviour**, 16 **Collapse Order**, 17 **Column Vertical Alignment** |
| Design | 5 **Background Color**, 6 **Background Image**, 7 **Background Image Alt Text**, 7 **Background Image Display**, 8 **Background Image Size**, 9 **Background Image Opacity**, 10 **Border Color**, 11 **Border Thickness**, 12 **Border Radius**, 20 **Box Shadow**, 25 **Box Shadow Hover**, 30 **Full Height** |

## Adding the Field's Value to the Row

The `siteorigin_panels_row_style_attributes` filter adds the field's value to the row's HTML. This example adds a `parallax` class to rows with **Parallax** enabled:

```php
function custom_row_style_attributes( $attributes, $args ) {
	if ( ! empty( $args['parallax'] ) ) {
		array_push( $attributes['class'], 'parallax' );
	}

	return $attributes;
}

add_filter( 'siteorigin_panels_row_style_attributes', 'custom_row_style_attributes', 10, 2 );
```

The filter passes two arrays. `$args` holds the row's saved style values, and `$attributes` holds the attributes of the row's style wrapper element:

- To add a class, add the class name to `$attributes['class']`.
- To add an inline style, such as `text-align: center;`, add it to `$attributes['style']`.
- To add an attribute, add a key and value to `$attributes`. For example, `$attributes['data-video-id'] = $args['video-id'];` adds `data-video-id="9"` to the row's style wrapper element when the row's `video-id` value is 9.

## Changing Saved Styles Before They Load

Two filters change the saved styles of a row, column or widget before Page Builder loads them into the style form, so you can migrate old values to new fields. Page Builder stores the changes only when the user saves the page or widget. `siteorigin_panels_general_current_styles` runs for every type, and `siteorigin_panels_general_current_styles_{type}` runs for one type, where `{type}` is `row`, `cell` or `widget`. Both filters pass these arguments:

- `$current`: the current style values.
- `$post_id`: the ID of the post being edited. The value is empty outside the post editor.
- `$type`: the type of item. Only `siteorigin_panels_general_current_styles` passes this argument.
- `$args`: the builder arguments.

## Showing Fields Conditionally With JavaScript

Page Builder triggers the `setup_style_fields` event on the document when it sets up a style form. Use it to start JavaScript for a custom field, such as a date picker, or to show and hide fields. The event passes the style form's view.

This example shows the field with the ID `example` only when the `toggle_example` checkbox is checked:

```javascript
( function( $ ) {
	$( document ).on( 'setup_style_fields', function( e, view ) {
		var exampleField = view.$el.find( '.so-field-example' );

		view.$el.find( '.so-field-toggle_example input[type="checkbox"]' ).on( 'change', function() {
			// If the toggle example field is checked, show the field.
			$( this ).is( ':checked' ) ? exampleField.show() : exampleField.hide();
		} ).trigger( 'change' );
	} );
} )( jQuery );
```
