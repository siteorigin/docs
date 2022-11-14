# Stopping Output of Row or Widget
There are situations where you may wish to stop the output of a row or widget, and you can do this using the `siteorigin_panels_output_row` and `siteorigin_panels_output_widget` filters. While it's possible to remove them using other methods (such as filtering the panels_data array), those methods will require additional adjustments to account for Page Builder CSS to prevent ID mismatches.

### Filter: siteorigin_panels_output_row
This filter has the following arguments:

* `$output` controls whether to output the row. Default is true.
* `$row` is the current row instance.
* `$ri` is the Page Builder ID of the current row. Please note that this is a zero-based index. This means that instead of starting at 1, the first item ID is 0.
* `$post_id` the post ID of the current post.

The following snippet will prevent the output of a row when the Row Label is set to `test`.

```php
add_filter( 'siteorigin_panels_output_row', function( $output, $row, $ri, $panels_data, $post_id ) {
	if ( ! empty( $row['label'] ) && $row['label'] == 'test' ) {
		$output = false;
	}

	return $output;
}, 10, 5 );
```

### Filter: siteorigin_panels_output_widget
This filter has the following arguments:

* `$output` controls whether to output the row. Default is true.
* `$widget` is the current widget instance.
* `$ri` is the Page Builder ID of the current row. 
* `$ci` is the Page Builder ID of the current cell.
* `$wi` is the Page Builder ID of the current widget.
* `$panels_data` the data of the currently rendering layout.
* `$post_id` the post ID of the current post.

Please note that Page Builder IDs have a zero-based index. This means that instead of starting at 1, the first item is already 0.

The following snippet will prevent the output of the Archive widget.

```php
add_filter( 'siteorigin_panels_output_widget', function( $output, $widget, $ri, $ci, $wi, $panels_data, $post_id ) {
	if ( $widget['panels_info']['panels_info'] == 'WP_Widget_Archives' ) {
		$output = false;
	}

	return $output;
}, 10, 7 );
```
