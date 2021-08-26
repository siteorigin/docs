# Connecting Multiple Media Form Field to a Repeater

A multiple media form field can be connected to a repeater to allow users to bulk add images. Any images added using this field won't be stored in the field directly; instead, they'll be stored in the repeater directly. This functionality currently only supports adding images to media fields inside repeaters at this time.

## Example

```php
$form_options = array(
	'example_multiple_media' => array(
		'type' => 'multiple_media',
		'label' => __( 'Example repeater multiple media', 'so-widgets-test' ),
		'repeater' => array(
			'field' => 'images', // The ID of the repeater form field.
			'setting' => 'image', // The media form field inside of the repeater that'll be set when the user adds new images.
		),
	),
	'images' => array(
		'type' => 'repeater',
		'label' => __( 'Images', 'so-widgets-test' ),
		'item_name'  => __( 'Image', 'so-widgets-test' ),
		'fields' => array(
			'image' => array(
				'type' => 'media',
				'label' => __( 'Image', 'so-widgets-test' ),
				'library' => 'image',
				'fallback' => true,
			),
		),
	),
);
```

#### Test Plugin

We've prepared a test plugin for you to try. You can [download it by clicking here](https://siteorigin.com/wp-content/uploads/2021/08/siteorigin-multiple-media-repeater.zip). Once downloaded, navigate to **Plugins > Add New** and upload **siteorigin-multiple-media-repeater.zip**. When prompted, activate the **SiteOrigin - Multiple Media Repeater Test Widget** plugin.

Once installed, navigate to **Plugins > SiteOrigin Widgets** and activate the **SiteOrigin Multiple Media Repeater** widget. Open any Page Builder powered page and add the **SiteOrigin Multiple Media Repeater** widget to your page.
