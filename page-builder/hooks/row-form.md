### Filter: siteorigin_panels_default_row_columns

It's possible to override the default number of row columns, and their weight (width), by using the `siteorigin_panels_default_row_columns` filter. This filter accepts a multidimensional array with each item being treated as a column. Each item must contain a weight item which is used to size the column. Weight is decimal based so 0.30 is equivalent to 30% of the row.

The following snippet will allow you to set the default:

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

### Filter: siteorigin_panels_row_column_count_input

It's possible to adjust the Row Column input markup by using the `siteorigin_panels_row_column_count_input` filter. This will allow alter the column number present in the input field. It doesn't however allow you to alter the default row columns, that's done using `siteorigin_panels_default_row_columns` filter.

The following snippet will column the column field to 4.

```php
add_filter( 'siteorigin_panels_row_column_count_input', function( $input ) {
	return '<input type="number" min="1" max="12" name="cells" class="so-row-field" value="4" />';
} );
```
