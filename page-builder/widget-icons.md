# Widget Icons

Widget icons are just class names that are displayed as divs next to widgets in Page Builder. These are a great way to give your users a quick visual representation of your widget.

The easiest thing to use for your icons is the official WordPress icon set, [Dashicons](http://calebserna.com/dashicons-cheatsheet/), but you can quite easily add your own icons.

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

This works in a similar way to how you'd assign [widget groups](./widget-groups.md). You use the `siteorigin_widgets` filter to alter the widgets icons.

```php
function mytheme_add_widget_icons($widgets){
	$widgets['My_Widget']['icon'] = 'dashicons dashicons-wordpress';
	return $widgets;
}
add_filter('siteorigin_panels_widgets', 'mytheme_add_widget_icons');
```
