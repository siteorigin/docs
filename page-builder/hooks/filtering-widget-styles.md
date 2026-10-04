# Filtering Widget Options

From Page Builder 2.12.3, the `siteorigin_panels_widget_style_fields` filter changes the widget style fields for one widget at a time. Its third argument, `$args`, holds the builder arguments. When a user edits a widget, `$args['widget']` holds the widget's class. When Page Builder builds its field cache, `$args` is `false`, so check that `$args['widget']` is set before you use it, or PHP shows a notice.

## Example

This example removes the **Widget ID** field from the **Attributes** group when a user edits the Archives widget:

```php
add_filter(
	'siteorigin_panels_widget_style_fields',
	function ( $fields, $post_id, $args ) {
		if ( isset( $args['widget'] ) && $args['widget'] == 'WP_Widget_Archives' ) {
			unset( $fields['id'] );
		}
		return $fields;
	},
	10,
	3
);
```
