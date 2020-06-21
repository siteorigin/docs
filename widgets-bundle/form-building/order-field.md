# Builder
The order form field allows users to reorder the listed options. The most common usage of this field is using the order field to control the output of widget contents.

## Example
```php
$form_options = array(
	'ordering' => array(
		'type' => 'order',
		'label' => __( 'Element Order', 'widget-form-fields-text-domain' ),
		'options' => array(
			'section' => __( 'Section', 'widget-form-fields-text-domain' ),
			'divider' => __( 'Content', 'widget-form-fields-text-domain' ),
			'other section' => __( 'Other Section', 'widget-form-fields-text-domain' ),
		),
		'default' => array( 'section', 'divider', 'other section' ),
	),
);
```

### Rendering the field
You can use `foreach` and `switch` to run through the through the `$instance['ordering']` `array`. An example of this is:

```php
foreach( $instance['ordering' as $item ) {
	switch( $item ) {
		case 'section' :
			// output here
			break;

		case 'divider' :
			// divider here
			echo '<hr>';
			break;

		case 'other section' :
			// output other section
			break;
	}
}
````
