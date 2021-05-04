# Filtering Page Builder HTML structure

By default, Page Builder gives you all the HTML you'll likely need to customize the look and feel of your layout. There are times, however, that you'll need to add your own HTML, classes or styles. You'll find most of these filters in the [siteorigin\_panels\_render](https://github.com/siteorigin/siteorigin-panels/blob/master/siteorigin-panels.php#L738).

### Before and After Content

Page builder gives you a pair of filters, `siteorigin_panels_before_content` and `siteorigin_panels_after_content` that let you add extra content before and after a layout. By default, these filters both output empty strings, but you can add what ever you need.

These filters both take 3 arguments. An empty string, which will eventually be echoed by Page Builder, `$panels_data` and `$post_id`.

They're called as follows.

```php
echo apply_filters( 'siteorigin_panels_before_content', '', $panels_data, $post_id );
// Page Builder content is rendered here
echo apply_filters( 'siteorigin_panels_after_content', '', $panels_data, $post_id );
```

### Before and After Rows

Just like before and after content, these filters give you a chance to add raw HTML before and after indivdual rows.

```php
echo apply_filters( 'siteorigin_panels_before_row', '', $panels_data['grids'][$gi], $grid_attributes );
// Row is generated here
echo apply_filters( 'siteorigin_panels_after_row', '', $panels_data['grids'][$gi], $grid_attributes );
```


### Inside Rows Before and After
///

You're able to output additional content inside of the rows and before/after the cells have been added. 

```php
// Row Container
echo apply_filters( 'siteorigin_panels_inside_row_before', '', $row );
// Cells
echo apply_filters( 'siteorigin_panels_inside_row_after', '', $row );
// Row Container End
```

### Inside Cells Before and After
///

Like with the Inside Before After Rows, you're able to output additional markup inside of the cells. This will give you chance to wrap the widgets added to the cell with addiitonal markup before/after the widget has rendered.

```php
// Row Container
// Cell Container
echo apply_filters( 'siteorigin_panels_inside_cell_before', '', $cell );
// Widgets added to cell
echo apply_filters( 'siteorigin_panels_inside_cell_after', '', $cell )
// Cell Container End
// Row Container End
```


### Row and Cell Styles

Page Builder has a few ways for you to add classes and CSS attributes to style wrappers. These wrappers are designed to give you a way to add visual styling to your Page Builder elements.

This is how the filters are called for rows.

```php
$grid_classes = apply_filters( 'siteorigin_panels_row_classes', array('panel-grid'), $panels_data['grids'][$gi] );
$grid_attributes = apply_filters( 'siteorigin_panels_row_attributes', array(
	'class' => implode( ' ', $grid_classes ),
	'id' => 'pg-' . $post_id . '-' . $gi
), $panels_data['grids'][$gi] );
```

The first filter `siteorigin_panels_row_classes` lets you add custom styling. The second is `siteorigin_panels_row_attributes` lets you add HTML and CSS attributes as an associative array. So you might have a function as follows.

```php
function myplugin_filter_row_attributes($attributes, $grid){
	// Look in $grid['style'] and from that add 
	if(empty($attributes['style'])) $attributes['style'] = array();
	$attributes['style']['background'] = '#00FF00';
}
add_filter('siteorigin_panels_row_attributes','myplugin_filter_row_attributes', 10, 2);
```

Dealing with cell styles is similar.

```php
// Themes can add their own styles to cells
$cell_classes = apply_filters( 'siteorigin_panels_row_cell_classes', array('panel-grid-cell'), $panels_data );
$cell_attributes = apply_filters( 'siteorigin_panels_row_cell_attributes', array(
	'class' => implode( ' ', $cell_classes ),
	'id' => 'pgc-' . $post_id . '-' . $gi  . '-' . $ci
), $panels_data );
```
