### Filtering Widget Settings Based on Widget Being Edited

As of SiteOrigin Page Builder `2.12.2`, it's possible to filter what Widget Styles are available on widget by widget basis. This is done using the `siteorigin_panels_widget_style_fields` filter which has an optional parameter called `$args` which contains additional information about the current styles. When editing a widget, `$args['widget']` will be set to the currently active Widget Class.

Due to `$args['widget']` not always being present, it's recommended you check that array key exists prior to using it to avoid a PHP notice.

#### Example

The following snippet will remove the Widget ID field in the Attributes settings group when editing the Archives Widget.

```php
add_filter( 'siteorigin_panels_widget_style_fields', function( $fields, $post_id, $args ) {
	if ( isset( $args['widget'] ) && $args['widget'] == 'WP_Widget_Archives' ) {
		unset( $fields['id'] );
	}
	return $fields;
}, 10, 3 );
```
