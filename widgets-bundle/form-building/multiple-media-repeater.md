# Connecting a Multiple Media Field to a Repeater

A multiple media field connected to a repeater lets users add many images at once. Each image the user selects becomes a new repeater item, and the multiple media field itself stores nothing. The repeater's target field must be a media field.

## Example

```php
$form_options = array(
	'example_multiple_media' => array(
		'type' => 'multiple_media',
		'label' => __( 'Example repeater multiple media', 'siteorigin-docs' ),
		'repeater' => array(
			'field' => 'images', // The ID of a repeater at the same level of the form.
			'setting' => 'image', // The media form field inside of the repeater that'll be set when the user adds new images.
		),
	),
	'images' => array(
		'type' => 'repeater',
		'label' => __( 'Images', 'siteorigin-docs' ),
		'item_name'  => __( 'Image', 'siteorigin-docs' ),
		'fields' => array(
			'image' => array(
				'type' => 'media',
				'label' => __( 'Image', 'siteorigin-docs' ),
				'library' => 'image',
				'fallback' => true,
			),
		),
	),
);
```

## Test Plugin

The [test plugin](https://siteorigin.com/wp-content/uploads/2021/08/siteorigin-multiple-media-repeater.zip) shows the connection in a working widget:

1. Download the plugin.
2. Go to **Plugins > Add Plugin**, click **Upload Plugin** and upload `siteorigin-multiple-media-repeater.zip`.
3. Activate the **SiteOrigin - Multiple Media Repeater Test Widget** plugin.
4. Go to **Plugins > SiteOrigin Widgets** and activate the **SiteOrigin Multiple Media Repeater** widget.
5. Add the **SiteOrigin Multiple Media Repeater** widget to a page in Page Builder.
