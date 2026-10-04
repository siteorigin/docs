# Overriding the Row Collapse Point

Page Builder stacks a row's columns when the screen is narrower than the **Mobile Width** in the Page Builder settings. The `siteorigin_panels_css_row_collapse_point` filter sets a different width for each row. The filter takes effect only when **Responsive Layout** is enabled in the Page Builder settings and the row has no **Collapse Behaviour** set. It passes four arguments:

- `$collapse_point`: the row's collapse point. Page Builder passes an empty value and uses the **Mobile Width** unless you return a width.
- `$row`: the row's data.
- `$ri`: the row's index in the layout. The first row is 0.
- `$panels_data`: the layout data.

## Example

This example sets the collapse point of the first row in a layout to 550 pixels:

```php
add_filter(
	'siteorigin_panels_css_row_collapse_point',
	function ( $collapse_point, $row, $ri, $panels_data ) {
		if ( $ri == 0 ) {
			$collapse_point = 550;
		}

		return $collapse_point;
	},
	10,
	4
);
```

## Adding a Collapse Point Setting to Rows

This example adds a **Row Collapse Point** field to each row's **Layout** settings, so users can set the collapse point of each row in Page Builder. It uses the `siteorigin_panels_row_style_fields` filter from [Filtering Custom Row Options](filtering-row-styles.md).

```php
// Add in collapse point input field.
add_filter(
	'siteorigin_panels_row_style_fields',
	function ( $fields ) {
		$fields['collapse_point'] = array(
			'name'        => __( 'Row Collapse Point', 'custom-text-domain' ),
			'type'        => 'number',
			'group'       => 'layout',
			'description' => sprintf( __( 'Row Collapse point. Default is %spx.', 'custom-text-domain' ), siteorigin_panels_setting( 'mobile-width' ) ),
			'priority'    => 11,
		);

		return $fields;
	},
	20
);

// Override the row collapse point as needed.
add_filter(
	'siteorigin_panels_css_row_collapse_point',
	function ( $collapse_point, $row ) {
		if ( ! empty( $row['style']['collapse_point'] ) ) {
			$collapse_point = $row['style']['collapse_point'];
		}

		return $collapse_point;
	},
	10,
	2
);
```
