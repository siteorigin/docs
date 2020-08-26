#Page Builder Widget Groups

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

Next you just need to make sure that you assign your widgets a group. You can either do this by filtering the widgets array using `siteorigin_panels_widgets` and add a `groups` attribute to each of your widgets, or you can add a `panels_groups` argument to the `$widget_options` argument of the `WP_Widget` constructor.

### Filtering Page Builder Widgets

```php
function mytheme_add_widget_icons($widgets){
	$widgets['My_Widget']['groups'] = array('mytheme');
	return $widgets;
}
add_filter('siteorigin_panels_widgets', 'mytheme_add_widget_icons');
```

#### Add a widget to the recommended Group
If you would like to add widgets to other groups, like recommended, and still keep the initial groups where the widget belonged to.
```php
function add_my_widget_into_recommended_group($widgets){
    
    $widgets['My_Widget']['groups'][] = 'recommended';
    return $widgets;
}

add_filter('siteorigin_panels_widgets', 'add_my_widget_into_recommended_group', 12);
```

#### Adding Multiple Widgets at once
It is easier to maintain all the recommended widgets for your project with a loop the same group(s).

```php
function set_so_widget_into_recommended_group($widgets){
    
    $widgets_classes_to_recommened = array(
        'My_Widget_1',
        'My_Widget_2'
    );
    
    foreach($widgets_classes_to_recommened as $index => $widget_class){
        $widgets[$widget_class]['groups'][] = 'recommended';
    }
    return $widgets;
}
add_filter('siteorigin_panels_widgets', 'set_so_widget_into_recommended_group', 12);
```

#### Maintain Standard Widget Ordering
SiteOrigin widgets bundle plugin add widgets' groups on the priority `11`. If you set a value lesser or equal `<=` to `11`, your groups will be overwritten by the SiteOrigin Widgets bundle filter. When you would like to organize the widgets in your project, you must add your function after `11`.
`add_filter('siteorigin_panels_widgets', 'your_function', 12);`



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
