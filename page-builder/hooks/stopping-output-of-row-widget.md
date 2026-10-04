# Stopping Row and Widget Output

The `siteorigin_panels_output_row` and `siteorigin_panels_output_widget` filters stop Page Builder from outputting a row or a widget. Use them in place of removing the row or widget from the `panels_data` array: Page Builder then still generates the layout's CSS for the full layout, so the IDs in the CSS match the rows and widgets on the page.

## Stopping a Row

The `siteorigin_panels_output_row` filter passes five arguments:

- `$output`: whether Page Builder outputs the row. The default is `true`.
- `$row`: the row's data.
- `$ri`: the row's index in the layout. The first row is 0.
- `$panels_data`: the data of the layout Page Builder is rendering.
- `$post_id`: the ID of the current post.

This example stops the output of any row labeled "test":

```php
add_filter(
	'siteorigin_panels_output_row',
	function ( $output, $row, $ri, $panels_data, $post_id ) {
		if ( ! empty( $row['label'] ) && $row['label'] == 'test' ) {
			$output = false;
		}

		return $output;
	},
	10,
	5
);
```

## Stopping a Widget

The `siteorigin_panels_output_widget` filter passes seven arguments:

- `$output`: whether Page Builder outputs the widget. The default is `true`.
- `$widget`: the widget's instance.
- `$ri`: the index of the widget's row in the layout.
- `$ci`: the index of the widget's column in the row.
- `$wi`: the index of the widget in the column.
- `$panels_data`: the data of the layout Page Builder is rendering.
- `$post_id`: the ID of the current post.

Each index starts at 0. This example stops the output of every Archives widget:

```php
add_filter(
	'siteorigin_panels_output_widget',
	function ( $output, $widget, $ri, $ci, $wi, $panels_data, $post_id ) {
		if ( $widget['panels_info']['class'] == 'WP_Widget_Archives' ) {
			$output = false;
		}

		return $output;
	},
	10,
	7
);
```
