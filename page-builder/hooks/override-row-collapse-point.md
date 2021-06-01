The `siteorigin_panels_css_row_collapse_point` filter will allow you to override the global Page Builder Mobile Width on a row by row basis.

#### Parameters

**collapse_point**

collapse_point is the collapse_point of the current row. This parameter is empty by default and the global Page Builder Mobile Width is used instead.

**row**

The instance of the current row instance.

**ri**

The row index of the current row. Please note that the row index is not zero-based so the first row index is 1.

**$panels_data**

The current Page Builder instance.

### Example

The following example will override the collapse point of the first row in the current Page Builder instance and set it to `550`.

```php
add_filter( 'siteorigin_panels_css_row_collapse_point', function( $collapse_point, $row, $ri, $panels_data ) {
	if ( $ri == 1 ) {
		$collapse_point = 550;
	}

	return $collapse_point;
}, 10, 4 );
```

### Dedicated Collapse Point Row Setting

The following snippet will allow you to set a Collapse Point on a row by row basis using the Page Builder interface. This snippet makes use of `siteorigin_panels_row_style_fields` filter which is outlined on the [Custom Row Options Hooks](custom-row-settings.md) page.

```php
// Add in collapse point input field.
add_filter( 'siteorigin_panels_row_style_fields', function( $fields ) {
	$fields['collapse_point'] = array(
		'name'        => __( 'Row Collapse Point', 'siteorigin-panels' ),
		'type'        => 'number',
		'group'       => 'layout',
		'description' => sprintf( __( 'Row Collapse point. Default is %spx.', 'custom-text-domain' ), siteorigin_panels_setting( 'mobile-width' ) ),
		'priority'    => 11,
	);

	return $fields;
}, 20 );

// Override the rows collapse point as needed.
add_filter( 'siteorigin_panels_css_row_collapse_point', function( $collapse_point, $row ) {
	if ( ! empty( $row['style']['collapse_point'] ) ) {
		$collapse_point = $row['style']['collapse_point'];
	}

	return $collapse_point;
}, 10, 2 );
```
