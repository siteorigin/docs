# Image Size

The image size field allows the user to specifically select a desired [image size](https://developer.wordpress.org/reference/functions/add_image_size/). This gives the user option of using a larger, or smaller image based on the context the widget is being used in rather than enforcing a specific size of the image (for example, `thumbnail` or `full`).

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

### Outputting with selected image size

The `image_size` selection returns the selected image size name, for example `full`, `thumbnail` or another registered size, so to render an image with this selection, while also accounting for no selection, we can use [wp_get_attachment_image_src](https://developer.wordpress.org/reference/functions/wp_get_attachment_image_src/).
Here's some example code that can be used in your template:

```php
if( ! empty( $instance['image'] ) ) {
	$size = empty( $instance['image_size'] ) ? 'full' : $instance['image_size']; // Account for no image size selection
	$attachment = wp_get_attachment_image_src( $instance['image'], $size );
	if( !empty( $attachment ) ) {
		echo '<div style="width: 100%; height: 250px; background-image: url(' . sow_esc_url( $attachment[0] ) . ')"></div>';
	}
}
```

The above PHP will:
1. Check to ensure an image is set.
2. Check if an image size is set.
  - If no size is set, default to full.
  - If size is set, use `image_size`.
3. Fetch the image.
4. Ensure image exists.
5. Render Image as a full width div with 250px height with the image set as the background.

If your media field sets `'fallback' => true`, use `siteorigin_widgets_get_attachment_image_src( $instance['image'], $size, $instance['image_fallback'] ?? false )` in place of `wp_get_attachment_image_src()`. It returns the fallback URL when no attachment is selected.
