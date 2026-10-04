# Image Size Field

The image size field lets users choose one of the site's [image sizes](https://developer.wordpress.org/reference/functions/add_image_size/), so a widget can show a larger or smaller image to suit where it sits.

## Example

```php
$form_options = array(
	'image' => array(
		'type' => 'media',
		'library' => 'image',
		'label' => __( 'Background Image', 'siteorigin-docs' ),
	),
	'image_size' => array(
		'type' => 'image-size',
		'label' => __( 'Background Image size', 'siteorigin-docs' ),
	)
);
```

## Outputting the Selected Size

The field saves the name of the selected size, such as `full`, `thumbnail` or another registered size. Pass it to [`wp_get_attachment_image_src()`](https://developer.wordpress.org/reference/functions/wp_get_attachment_image_src/), and use `full` when the user hasn't chosen a size:

```php
if( ! empty( $instance['image'] ) ) {
	$size = empty( $instance['image_size'] ) ? 'full' : $instance['image_size']; // Account for no image size selection
	$attachment = wp_get_attachment_image_src( $instance['image'], $size );
	if( !empty( $attachment ) ) {
		echo '<div style="width: 100%; height: 250px; background-image: url(' . sow_esc_url( $attachment[0] ) . ')"></div>';
	}
}
```

If your media field sets `'fallback' => true`, use `siteorigin_widgets_get_attachment_image_src()` in place of `wp_get_attachment_image_src()`. The function returns the fallback URL when no attachment is selected, so it replaces the `! empty( $instance['image'] )` check. Output a fallback URL in an `src` attribute, because `esc_url()` doesn't make a URL safe inside inline CSS:

```php
$size = empty( $instance['image_size'] ) ? 'full' : $instance['image_size'];
$attachment = siteorigin_widgets_get_attachment_image_src(
	$instance['image'],
	$size,
	! empty( $instance['image_fallback'] ) ? $instance['image_fallback'] : false
);
if ( ! empty( $attachment ) ) {
	echo '<img src="' . sow_esc_url( $attachment[0] ) . '" alt="">';
}
```
