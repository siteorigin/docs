# Filtering Page Builder HTML Structure

Page Builder outputs classes and wrapper elements for every layout, row, column and widget. To add your own HTML, classes or attributes, use the filters in the [`SiteOrigin_Panels_Renderer`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/renderer.php) class, which `siteorigin_panels_render()` calls.

## Before and After the Layout

The `siteorigin_panels_before_content` and `siteorigin_panels_after_content` filters add HTML before and after a layout. Both start with an empty string, which Page Builder echoes, and also pass `$panels_data` and `$post_id`:

```php
echo apply_filters( 'siteorigin_panels_before_content', '', $panels_data, $post_id );
// Page Builder content is rendered here.
echo apply_filters( 'siteorigin_panels_after_content', '', $panels_data, $post_id );
```

## Layout Wrapper

Page Builder wraps each layout in a `div`. The `siteorigin_panels_layout_classes` and `siteorigin_panels_layout_attributes` filters change the wrapper's classes and attributes:

```php
$layout_classes    = apply_filters( 'siteorigin_panels_layout_classes', array( 'panel-layout' ), $post_id, $panels_data );
$layout_attributes = apply_filters(
	'siteorigin_panels_layout_attributes',
	array(
		'id'    => 'pl-' . $post_id,
		'class' => implode( ' ', $layout_classes ),
	),
	$post_id,
	$panels_data
);
```

## Before and After Rows

The `siteorigin_panels_before_row` and `siteorigin_panels_after_row` filters add HTML before and after each row. `$row` is the row's layout data, including its `style` and `cells`:

```php
echo apply_filters( 'siteorigin_panels_before_row', '', $row, $row_attributes );
// Row is generated here.
echo apply_filters( 'siteorigin_panels_after_row', '', $row, $row_attributes );
```

## Inside Rows

The `siteorigin_panels_inside_row_before` and `siteorigin_panels_inside_row_after` filters add HTML inside each row, before and after its columns:

```php
// Row Container.
echo apply_filters( 'siteorigin_panels_inside_row_before', '', $row );
// Cells.
echo apply_filters( 'siteorigin_panels_inside_row_after', '', $row );
// Row Container End.
```

## Before and After Columns

The `siteorigin_panels_before_cell` and `siteorigin_panels_after_cell` filters add HTML before and after each column:

```php
echo apply_filters( 'siteorigin_panels_before_cell', '', $cell, $cell_attributes );
// Cell is generated here.
echo apply_filters( 'siteorigin_panels_after_cell', '', $cell, $cell_attributes );
```

## Inside Columns

The `siteorigin_panels_inside_cell_before` and `siteorigin_panels_inside_cell_after` filters add HTML inside each column, before and after its widgets, so you can wrap a column's widgets in your own markup:

```php
// Row Container
// Cell Container
echo apply_filters( 'siteorigin_panels_inside_cell_before', '', $cell );
// Widgets Added To Cell
echo apply_filters( 'siteorigin_panels_inside_cell_after', '', $cell );
// Cell Container End
// Row Container End
```

## Inside Widgets

The `siteorigin_panels_inside_widget_before` and `siteorigin_panels_inside_widget_after` filters add HTML inside each widget's wrapper, before and after the widget's content. `$widget_info` is the widget's `panels_info` array:

```php
$args['before_widget'] .= apply_filters( 'siteorigin_panels_inside_widget_before', '', $widget_info );
$args['after_widget']   = apply_filters( 'siteorigin_panels_inside_widget_after', '', $widget_info ) . $args['after_widget'];
```

## Row and Column Classes and Attributes

Rows and columns have filters for the classes and attributes of their wrappers. Page Builder calls the row filters like this:

```php
$row_classes    = apply_filters( 'siteorigin_panels_row_classes', $row_classes, $row );
$row_attributes = apply_filters(
	'siteorigin_panels_row_attributes',
	array(
		'id'    => 'pg-' . $post_id . '-' . $ri,
		'class' => implode( ' ', $row_classes ),
	),
	$row
);
```

`siteorigin_panels_row_classes` adds classes to the row, and `siteorigin_panels_row_attributes` adds HTML attributes as an associative array. This example adds a background color to the row's `style` attribute:

```php
function myplugin_filter_row_attributes( $attributes, $row ) {
	// Look in $row['style'] and from that add your own attributes.
	// The style attribute is a CSS string.
	$attributes['style'] = ( ! empty( $attributes['style'] ) ? $attributes['style'] . '; ' : '' ) . 'background: #00FF00';

	return $attributes;
}
add_filter( 'siteorigin_panels_row_attributes', 'myplugin_filter_row_attributes', 10, 2 );
```

The column filters work the same way:

```php
// Themes can add their own styles to cells.
$cell_classes    = apply_filters( 'siteorigin_panels_cell_classes', $cell_classes, $cell );
$cell_attributes = apply_filters(
	'siteorigin_panels_cell_attributes',
	array(
		'id'    => 'pgc-' . $post_id . '-' . $ri . '-' . $ci,
		'class' => implode( ' ', $cell_classes ),
	),
	$cell
);
```

After these filters, Page Builder also runs the older `siteorigin_panels_row_cell_classes` and `siteorigin_panels_row_cell_attributes` filters, with `$panels_data` as the second argument and `$cell` as the third. Use `siteorigin_panels_cell_classes` and `siteorigin_panels_cell_attributes` in new code.

## Stopping Row and Widget Output

The `siteorigin_panels_output_row` and `siteorigin_panels_output_widget` filters stop Page Builder from outputting a row or a widget. Row indexes start at 0. [Stopping Row and Widget Output](stopping-output-of-row-widget.md) lists each filter's arguments.

```php
// Prevent the first row from outputting.
add_filter(
	'siteorigin_panels_output_row',
	function ( $output, $row, $ri, $panels_data, $post_id ) {
		if ( $ri === 0 ) {
			$output = false;
		}
		return $output;
	},
	10,
	5
);

// Prevent the Archive Widget from outputting.
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
