# Page Builder Widget Groups

Widget groups give you a way to keep your widgets organized within the **Add Widget** dialog.

![Widget Groups](./images/widget-groups.png)

You can add your own groups using the `siteorigin_panels_widget_dialog_tabs` filter. You need to add tabs and assign these tabs a group filter.

```php
function mytheme_add_widget_tabs($tabs) {
	$tabs[] = array(
		'title' => __('My Tab', 'mytheme'),
		'filter' => array(
			'groups' => array('mytheme')
		)
	);
	
	return $tabs;
}
add_filter('siteorigin_panels_widget_dialog_tabs', 'mytheme_add_widget_tabs', 20);
```

Next you just need to make sure that you assign your widgets a group. You can either do this by filtering the widgets array using `siteorigin_panels_widgets` and add a `groups` attribute to each of your widgets, or you can add a `panels_groups` argument to the `$widget_options` argument of the `WP_Widget` constructor. The keys of the widgets array are the widgets' PHP class names.

### Filtering Page Builder Widgets

```php
function mytheme_add_widget_groups($widgets){
	if ( isset( $widgets['My_Widget'] ) ) {
		$widgets['My_Widget']['groups'] = array('mytheme');
	}
	return $widgets;
}
add_filter('siteorigin_panels_widgets', 'mytheme_add_widget_groups');
```

#### Add a Widget to the Recommended Group

You can set widgets to the recommended group by setting the group to `recommended`.

```php
function add_my_widget_into_recommended_group( $widgets ) {
    
    if ( isset( $widgets['My_Widget'] ) ) {
        $widgets['My_Widget']['groups'][] = 'recommended';
    }
    return $widgets;
}

add_filter( 'siteorigin_panels_widgets', 'add_my_widget_into_recommended_group', 12 );
```

#### Maintain Standard Widget Ordering

The Widgets Bundle sets the groups of its own widgets at priority 11, and Page Builder sets the groups of WordPress core widgets at priority 10. To change the groups of those widgets, use priority 12 or higher. For example:

`add_filter( 'siteorigin_panels_widgets', 'your_function', 12 );`

#### Adding Multiple Widgets at once

It's possible to add multiple widgets at a time using a standard `foreach` loop. No restrictions are imposed on how many widgets you can add at a time.

```php
function set_so_widget_into_recommended_group( $widgets ) {
    
    $recommended_classes = array(
        'My_Widget_1',
        'My_Widget_2'
    );
    
    foreach ( $recommended_classes as $widget_class ) {
        if ( isset( $widgets[ $widget_class ] ) ) {
            $widgets[ $widget_class ]['groups'][] = 'recommended';
        }
    }
    return $widgets;
}
add_filter( 'siteorigin_panels_widgets', 'set_so_widget_into_recommended_group', 12 );
```

#### Caching

Page Builder caches the widget list in the `siteorigin_panels_widgets` transient for 10 minutes. It caches its own dialog tabs, including the **Recommended Widgets** tab, in the `siteorigin_panels_widget_dialog_tabs` transient for a day. The **Recommended Widgets** tab only shows if a widget was in the `recommended` group when Page Builder built that transient. Delete both transients while you develop to see your changes right away.

### Widgets Argument

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
				'panels_groups' => array('mytheme')
			)
		);
	}
}
```
