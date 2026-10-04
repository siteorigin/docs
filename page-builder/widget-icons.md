# Widget Icons

Page Builder shows an icon beside each widget in the **Add Widget** dialog, so users can spot a widget at a glance. A widget icon is a set of class names that Page Builder adds to a `span`, and a widget without an icon gets `dashicons dashicons-admin-generic`. Use [Dashicons](https://developer.wordpress.org/resource/dashicons/), the WordPress icon set, or your own icon classes with CSS that you load in the admin with `admin_enqueue_scripts`.

## Setting the Icon in Your Widget

Add a `panels_icon` argument to the `$widget_options` argument of the `WP_Widget` constructor:

```php
class Foo_Widget extends WP_Widget {

	/**
	 * Register widget with WordPress.
	 */
	function __construct() {
		parent::__construct(
			'foo_widget', // Base ID
			__( 'Widget Title', 'text_domain' ), // Name
			array(
				'description' => __( 'A Foo Widget', 'text_domain' ),
				'panels_icon' => 'dashicons dashicons-wordpress',
			)
		);
	}
}
```

## Setting the Icon With a Filter

The `siteorigin_panels_widgets` filter changes the icon of any widget, including widgets you didn't write. It works like [Page Builder Widget Groups](./widget-groups.md): the array key is the widget's PHP class name.

```php
function mytheme_add_widget_icons( $widgets ) {
	if ( isset( $widgets['My_Widget'] ) ) {
		$widgets['My_Widget']['icon'] = 'dashicons dashicons-wordpress';
	}
	return $widgets;
}
add_filter( 'siteorigin_panels_widgets', 'mytheme_add_widget_icons' );
```

The Widgets Bundle sets the icons of its own widgets at priority 11. To change the icon of a Widgets Bundle widget, add your filter at priority 12 or higher.

Page Builder caches the widget list in the `siteorigin_panels_widgets` transient for 10 minutes. Delete the transient while you develop to see your changes right away.
