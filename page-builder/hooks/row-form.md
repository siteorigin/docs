# Filtering the Row Form

## Default Row Columns

New rows have two columns with a weight of 0.5 each. The `siteorigin_panels_default_row_columns` filter changes the number of columns in a new row and the width of each column. The filter returns an array with one item per column, and each item sets the column's `weight`, its share of the row's width as a decimal: 0.30 is 30% of the row.

This example gives new rows three equal columns:

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

## Column Count Field

The `siteorigin_panels_row_column_count_input` filter changes the HTML of the column count field in the row form, including the number the field shows. To change the columns of a new row, use `siteorigin_panels_default_row_columns` instead.

This example sets the column count field to 4:

```php
add_filter( 'siteorigin_panels_row_column_count_input', function( $input ) {
	return '<input type="number" min="1" max="12" name="cells" id="so-row-count-input" class="so-row-field" value="4" />';
} );
```
