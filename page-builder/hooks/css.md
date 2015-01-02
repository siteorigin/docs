#Page Builder CSS Hooks

Page Builder has several hooks for filtering CSS output. This can be useful if you want to make adjustments to how Page Builder renders its content or if you're making changes to the general HTML structure using [Page Builder's HTML hooks](./html.css).

## Filtering CSS Values

Page Builder has several filters along the way to change various aspects of the Page Builder layout. You can find most of these in the [siteorigin\_panels\_generate\_css](https://github.com/siteorigin/siteorigin-panels/blob/master/siteorigin-panels.php#L579) function.

* `siteorigin_panels_css_cell_width` - This filter lets you change the percentage width of an individual cell. Values are given as a string, like `33.333%`. You can filter this to change the percent or the units. This filter gives you the following arguments.
	* `$width` - The width string.
	* `$grid` - The grid array. You can var_dump or use a debugger to see what's available to you here.
	* `$gi` The row index.
	* `$cell` The cell array.
	* `$ci` The cell index.
	* `$panels_data` The full $panels_data array. This will also have all the style values.
	* `$post_id`
* `siteorigin_panels_css_row_margin_bottom` - This filter lets you change the bottom margin of a row. This is given as a CSS string (with px units), so you can change both the value and the units. The filter is called using the following code - `apply_filters('siteorigin_panels_css_row_margin_bottom', $panels_margin_bottom.'px', $grid, $gi, $panels_data, $post_id)`
* `siteorigin_panels_css_row_gutter` - This is a string that represents the space between the cells of a given row. This is often referred to as the row gutter. It's called as follows - `apply_filters('siteorigin_panels_css_row_gutter', $settings['margin-sides'].'px', $grid, $gi, $panels_data);`

## CSS Builder Class

Before delving too deep into filtering CSS, you should have a basic understanding of the [CSS builder class](https://github.com/siteorigin/siteorigin-panels/blob/master/inc/css.php).

Page Builder uses the [CSS builder class](https://github.com/siteorigin/siteorigin-panels/blob/master/inc/css.php) to create responsive CSS. You can quite easily develop for Page Builder without knowing exactly how this class works, but understanding it might help you create some fairly advanced functionality.

Before outputting CSS for a given layout, Page Builder gives themes and plugins an opportuinity to filter the CSS builder class `SiteOrigin_Panels_Css_Builder`.

```php
// Let other plugins and components filter the CSS object.
$css = apply_filters('siteorigin_panels_css_object', $css, $panels_data, $post_id);
return $css->get_css();
```

#### Adding Row CSS

The CSS builder has a function called [add\_row\_css](https://github.com/siteorigin/siteorigin-panels/blob/master/inc/css.php#L47). This function takes the following arguments.

* `$li` - This is the ID of the layout. It's only used when `$specify_layout` is set to true or when `$ri` is false.
* `$ri` - The row index that this CSS is for. If you set this to false, the CSS will apply to all rows in the given layout.
* `$sub_selector` - This adds a sub selection CSS to the CSS. This is useful if you want to target HTML elements, like style wrappers, within the row.
* `$attributes` - This is an associative array of CSS attributes. More examples will follow.
* `$resolution` - The resolution that this CSS will take effect. The default value of 1920 is treated as all. Any value lower than this will take effect at that resolution.
* `$specify_layout` - If set to true, this CSS will apply specifically to the given layout.

To better understand this, lets take a look at how Page Builder itself uses this class.

```php
$css->add_row_css($post_id, $gi, '', array(
	'margin-left' => '-' . $margin_half,
	'margin-right' => '-' . $margin_half,
) );
```

So in this case, Page Builder is giving a negative margin to the row, represented by `$gi`. So it's assigning a negative margin value to the `margin-left` and `margin-right` attributes.

#### Adding Cell CSS

We wont go as in depth with the cell CSS function because it's very similar to the row CSS class. Adding CSS is done through the [add\_cell\_css](https://github.com/siteorigin/siteorigin-panels/blob/master/inc/css.php#L77) function. It's arguments are mostly the same as [add\_row\_css](https://github.com/siteorigin/siteorigin-panels/blob/master/inc/css.php#L47), we just have an extra `$ci` argument that specifies the 0 base cell index.

#### Filtering the CSS Builder.

Page Builder gives you an opportunity to filter the builder class right before 