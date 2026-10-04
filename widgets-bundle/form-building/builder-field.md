# Builder Field

The builder field puts a full [Page Builder](https://wordpress.org/plugins/siteorigin-panels/) layout inside a widget's form. The field needs Page Builder, both to edit the layout and to output it.

## Example

```php
$form_options = array(
	'page_builder' => array(
		'type' => 'builder',
		'label' => __( 'Page Builder', 'widget-form-fields-text-domain'),
	)
);
```

## Rendering the Field

`siteorigin_panels_render()` outputs the field's layout. It takes three arguments:

- `$post_id` (`int|string|bool`): the post ID, or `'home'`. For a builder field, pass an ID that is unique to the layout.
- `$enqueue_css` (`bool`): whether to enqueue the layout's CSS. Set it to `true`.
- `$panels_data` (`array`): the layout data, from `$instance['page_builder']`.

The function is in Page Builder's [`inc/functions.php`](https://github.com/siteorigin/siteorigin-panels/blob/develop/inc/functions.php). This example builds the ID from a hash of the layout, so builder fields with different layouts get different IDs:

```php
if( function_exists( 'siteorigin_panels_render' ) ) {
	$content_builder_id = substr( md5( json_encode( $instance['page_builder'] ) ), 0, 8 );
	echo siteorigin_panels_render( 'w'.$content_builder_id, true, $instance['page_builder'] );
}
else {
	esc_html_e( 'This widget requires Page Builder.', 'widget-form-fields-text-domain' );
}
```
