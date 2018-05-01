# Builder
The builder field is an entire [SiteOrigin Page Builder](https://wordpress.org/plugins/siteorigin-panels/) instance. As such, SiteOrigin Page Builder is required for this field to work in all instances (settings and output). 

## Example
```php
$form_options = array(
	'page_builder' => array(
		'type' => 'builder',
		'label' => __( 'Page Builder', 'widget-form-fields-text-domain'),
	)
);
```

### Rendering the field

`siteorigin_panels_render` will handle rendering the field value itself, but it does require some information to be able to proceed. `siteorigin_panels_render` has three relevant parameters for outputting this field:

- post_id: `int`, `string`, `bool - $post_id The Post ID or 'home'.
- enqueue_css: `bool` Should we also enqueue the layout CSS. This, for all intents and purposes, should rarely not be set to true.
* panels_data: `array` $panels_data This is where we pass the contents of $instance['page_builder']

For information on `siteorigin_panels_render`, please refer to the full source code [here](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/renderer.php#L268).

```php
if( function_exists( 'siteorigin_panels_render' ) ) {
	$content_builder_id = substr( md5( json_encode( $content ) ), 0, 8 );
	echo siteorigin_panels_render( 'w'.$content_builder_id, true, $instance['page_builder'] );
}
else {
	echo __( 'This widget requires Page Builder.', 'so-widgets-bundle' );
}
```
`$content_builder_id` doesn't need to be complex, but we recommend using a complex id to prevent any potential mismatches.
