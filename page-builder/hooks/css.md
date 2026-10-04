# Page Builder CSS Hooks

Page Builder's CSS hooks change the CSS it generates for each layout. Use them to adjust how Page Builder styles its rows and columns, or to style HTML you've changed with the hooks in [Filtering Page Builder HTML Structure](./html.md).

## Filtering CSS Values

Page Builder applies these filters as it generates a layout's CSS, in the [`SiteOrigin_Panels_Renderer::generate_css()`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/renderer.php) method that `siteorigin_panels_generate_css()` calls:

* `siteorigin_panels_css_cell_weight` changes the share of the row that a column takes, as a number from 0 to 1, such as `0.3333`. Page Builder turns the weight into the column's percentage width and subtracts the column's share of the row gutter. The filter passes these arguments:
	* `$weight`: the column's weight.
	* `$row`: the row's data. Use `var_dump()` or a debugger to see what it holds.
	* `$ri`: the row's index.
	* `$cell`: the column's data.
	* `$ci`: the column's index minus one, so the first column in a row is `-1`.
	* `$panels_data`: the layout data, including every style value.
	* `$post_id`: the ID of the post.
* `siteorigin_panels_css_row_margin_bottom` changes a row's bottom margin. The value is a CSS string with its unit, such as `30px`, so you can change the unit as well as the number. Page Builder calls it with `apply_filters( 'siteorigin_panels_css_row_margin_bottom', $settings['margin-bottom'] . 'px', $row, $ri, $panels_data, $post_id )`.
* `siteorigin_panels_css_row_gutter` changes a row's gutter, the space between its columns, as a CSS string. Page Builder calls it with `apply_filters( 'siteorigin_panels_css_row_gutter', $settings['margin-sides'] . 'px', $row, $ri, $panels_data )`.
* `siteorigin_panels_css_row_mobile_margin_bottom` changes a row's bottom margin on mobile, and takes the same arguments as `siteorigin_panels_css_row_margin_bottom`.

## The CSS Builder Class

Page Builder generates its responsive CSS with the [`SiteOrigin_Panels_Css_Builder`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/css-builder.php) class. Before it outputs a layout's CSS, Page Builder passes the class through the `siteorigin_panels_css_object` filter, so your theme or plugin can add its own rules:

```php
// Let other plugins and components filter the CSS object.
$css = apply_filters( 'siteorigin_panels_css_object', $css, $panels_data, $post_id, $layout_data );
return $css->get_css();
```

### Adding Row CSS

The [`add_row_css()`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/css-builder.php#L65) method adds CSS to a row. It takes these arguments:

* `$li`: the layout's ID. It forms part of the row selector, `#pg-{$li}-{$ri}`, and Page Builder uses it on its own when `$specify_layout` is `true` or `$ri` is `false`.
* `$ri`: the index of the row the CSS applies to. Set it to `false` to apply the CSS to every row in the layout, or pass a string to use as the row's HTML ID.
* `$sub_selector`: a selector inside the row, for elements such as the row's style wrapper.
* `$attributes`: an associative array of CSS properties and values.
* `$resolution`: the screen width the CSS applies to. The default, 1920, applies the CSS at every width. A lower number applies the CSS at that width and below, and a `"max:min"` string, such as `"1024:781"`, applies it to a range.
* `$specify_layout`: set to `true` to apply the CSS only to this layout.

Page Builder uses `add_row_css()` to give a row, `$ri`, its bottom margin:

```php
$css->add_row_css(
	$post_id,
	$ri,
	'',
	array(
		'margin-bottom' => $panels_margin_bottom,
	)
);
```

### Adding Column CSS

The [`add_cell_css()`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/css-builder.php#L104) method adds CSS to a column. It takes the same arguments as `add_row_css()`, plus `$ci`, the column's index in the row, starting at 0.

### Filtering the CSS Builder

This example uses the `siteorigin_panels_css_object` filter to give the first row of every layout a background color on screens 780 pixels wide and narrower:

```php
function mytheme_filter_css_object( $css, $panels_data, $post_id, $layout_data ) {
	$css->add_row_css(
		$post_id,
		0,
		'',
		array(
			'background-color' => '#f5f5f5',
		),
		780
	);

	return $css;
}
add_filter( 'siteorigin_panels_css_object', 'mytheme_filter_css_object', 10, 4 );
```
