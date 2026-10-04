# Order Field

The order field lets users drag a list of options into the order they want. Use it to let users choose the order of the parts of a widget.

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

## Rendering the Field

The field saves the option keys in their new order, so loop through `$instance['ordering']` and output each part in turn:

```php
foreach( $instance['ordering'] as $item ) {
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
```
