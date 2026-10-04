# Filtering Page Builder HTML Structure

By default, Page Builder gives you all the HTML you'll likely need to customize the look and feel of your layout. There are times, however, that you'll need to add your own HTML, classes or styles. You'll find most of these filters in the [`SiteOrigin_Panels_Renderer`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/renderer.php) class, which `siteorigin_panels_render()` calls.

### Before and After Content

Page builder gives you a pair of filters, `siteorigin_panels_before_content` and `siteorigin_panels_after_content` that let you add extra content before and after a layout. By default, these filters both output empty strings, but you can add what ever you need.

These filters both take 3 arguments. An empty string, which will eventually be echoed by Page Builder, `$panels_data` and `$post_id`.

They're called as follows.

```php
echo apply_filters( 'siteorigin_panels_before_content', '', $panels_data, $post_id );
// Page Builder content is rendered here.
echo apply_filters( 'siteorigin_panels_after_content', '', $panels_data, $post_id );
```

### Layout Wrapper

Page Builder wraps the whole layout in a `div`. The `siteorigin_panels_layout_classes` and `siteorigin_panels_layout_attributes` filters let you change its classes and attributes.

```php
$layout_classes = apply_filters( 'siteorigin_panels_layout_classes', array( 'panel-layout' ), $post_id, $panels_data );
$layout_attributes = apply_filters( 'siteorigin_panels_layout_attributes', array(
	'id'    => 'pl-' . $post_id,
	'class' => implode( ' ', $layout_classes ),
), $post_id, $panels_data );
```

### Before and After Rows

Just like before and after content, these filters give you a chance to add raw HTML before and after indivdual rows.

```php
echo apply_filters( 'siteorigin_panels_before_row', '', $row, $row_attributes );
// Row is generated here.
echo apply_filters( 'siteorigin_panels_after_row', '', $row, $row_attributes );
```

`$row` is the row's layout data, including its `style` and `cells`.

### Inside Rows Before and After

You can output additional content inside the rows and before/after the cells have been added.

```php
// Row Container.
echo apply_filters( 'siteorigin_panels_inside_row_before', '', $row );
// Cells.
echo apply_filters( 'siteorigin_panels_inside_row_after', '', $row );
// Row Container End.
```

### Before and After Cells

These filters add HTML before and after each cell.

```php
echo apply_filters( 'siteorigin_panels_before_cell', '', $cell, $cell_attributes );
// Cell is generated here.
echo apply_filters( 'siteorigin_panels_after_cell', '', $cell, $cell_attributes );
```

### Inside Cells Before and After

Like with the Inside Before After Rows, you can output additional markup inside the cells. This will allow you to wrap the widgets added to the cell with additional markup before/after the widget has rendered.

```php
// Row Container
// Cell Container
echo apply_filters( 'siteorigin_panels_inside_cell_before', '', $cell );
// Widgets Added To Cell
echo apply_filters( 'siteorigin_panels_inside_cell_after', '', $cell );
// Cell Container End
// Row Container End
```

### Inside Widgets Before and After

These filters add HTML inside each widget's wrapper, before and after the widget content. `$widget_info` is the widget's `panels_info` array.

```php
$args['before_widget'] .= apply_filters( 'siteorigin_panels_inside_widget_before', '', $widget_info );
$args['after_widget'] = apply_filters( 'siteorigin_panels_inside_widget_after', '', $widget_info ) . $args['after_widget'];
```

### Row and Cell Styles

Page Builder has a few ways for you to add classes and CSS attributes to style wrappers. These wrappers are designed to give you a way to add visual styling to your Page Builder elements.

This is how the filters are called for rows.

```php
$row_classes = apply_filters( 'siteorigin_panels_row_classes', $row_classes, $row );
$row_attributes = apply_filters( 'siteorigin_panels_row_attributes', array(
	'id'    => 'pg-' . $post_id . '-' . $ri,
	'class' => implode( ' ', $row_classes ),
), $row );
```

The first filter `siteorigin_panels_row_classes` lets you add classes. The second is `siteorigin_panels_row_attributes` lets you add HTML and CSS attributes as an associative array. So you might have a function as follows.

```php
function myplugin_filter_row_attributes( $attributes, $row ) {
	// Look in $row['style'] and from that add your own attributes.
	// The style attribute is a CSS string.
	$attributes['style'] = ( ! empty( $attributes['style'] ) ? $attributes['style'] . '; ' : '' ) . 'background: #00FF00';

	return $attributes;
}
add_filter('siteorigin_panels_row_attributes','myplugin_filter_row_attributes', 10, 2);
```

Dealing with cell styles is similar.

```php
// Themes can add their own styles to cells.
$cell_classes = apply_filters( 'siteorigin_panels_cell_classes', $cell_classes, $cell );
$cell_attributes = apply_filters( 'siteorigin_panels_cell_attributes', array(
	'id'    => 'pgc-' . $post_id . '-' . $ri . '-' . $ci,
	'class' => implode( ' ', $cell_classes ),
), $cell );
```

The older `siteorigin_panels_row_cell_classes` and `siteorigin_panels_row_cell_attributes` filters still run after these, with `$panels_data` as the second argument and `$cell` as the third. Use the filters above in new code.

### Prevent Output of Row or Widget

You can completely prevent a row or widget from outputting by using the `siteorigin_panels_output_row` and `siteorigin_panels_output_widget` filters. Row indexes start at 0.

```php
// Prevent the first row from outputting.
add_filter( 'siteorigin_panels_output_row', function( $output, $row, $ri, $panels_data, $post_id ) {
	if ( $ri === 0 ) {
		$output = false;
	}
	return $output;
}, 10, 5 );

// Prevent the Archive Widget from outputting.
add_filter( 'siteorigin_panels_output_widget', function( $output, $widget, $ri, $ci, $wi, $panels_data, $post_id ) {
	if ( $widget['panels_info']['class'] == 'WP_Widget_Archives' ) {
		$output = false;
	}

	return $output;
}, 10, 7 );

```