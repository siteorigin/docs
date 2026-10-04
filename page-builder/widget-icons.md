# Widget Icons

Widget icons are class names that Page Builder adds to a `span` beside each widget in the **Add Widget** dialog. Widgets without an icon get `dashicons dashicons-admin-generic`. These are a great way to give your users a quick visual representation of your widget.

The easiest thing to use for your icons is the official WordPress icon set, [Dashicons](https://developer.wordpress.org/resource/dashicons/), but you can also use your own icon classes with CSS you load in the admin using `admin_enqueue_scripts`.

### Widgets Argument

This is the easiest way to add icons to your widgets. You just need to include a `panels_icon` argument to the `$widget_options` argument of the `WP_Widget` constructor.

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
                'panels_icon' => 'dashicons dashicons-wordpress'
            )
        );
    }
}
```

### Filtering Page Builder Widgets

This works in a similar way to how you'd assign [widget groups](./widget-groups.md). Use the `siteorigin_panels_widgets` filter to change widget icons. The array key is the widget's PHP class name.

```php
function mytheme_add_widget_icons($widgets){
	if ( isset( $widgets['My_Widget'] ) ) {
		$widgets['My_Widget']['icon'] = 'dashicons dashicons-wordpress';
	}
	return $widgets;
}
add_filter('siteorigin_panels_widgets', 'mytheme_add_widget_icons');
```

The Widgets Bundle sets the icons of its own widgets at priority 11. To change the icon of a Widgets Bundle widget, add your filter at priority 12 or higher.

Page Builder caches the widget list in the `siteorigin_panels_widgets` transient for 10 minutes. Delete the transient while you develop to see your changes right away.
