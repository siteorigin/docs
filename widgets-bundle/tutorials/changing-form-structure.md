# Changing Form Structure

When you change the structure of a widget's form, such as moving fields into a section, saved widgets still hold data in the old structure. Override the `modify_instance()` method of `SiteOrigin_Widget` to convert an instance from the old structure to the new one.

## Updating the Form

This example groups a few of a widget's fields into a section. First, update the array that `get_widget_form()` returns.

The old form:

```php
$form_options = array(
	'employee_name'    => array(
		'type'  => 'text',
		'label' => __( 'Name', 'example-text-domain' ),
	),
	'employee_surname' => array(
		'type'  => 'text',
		'label' => __( 'Surname', 'example-text-domain' ),
	),
);
```

The new form:

```php
$form_options = array(
	'employee' => array(
		'type'   => 'section',
		'label'  => __( 'Employee', 'example-text-domain' ),
		'fields' => array(
			'name'    => array(
				'type'  => 'text',
				'label' => __( 'Name', 'example-text-domain' ),
			),
			'surname' => array(
				'type'  => 'text',
				'label' => __( 'Surname', 'example-text-domain' ),
			),
		),
	),
);
```

## Overriding `modify_instance()`

Override `modify_instance()` to move the old values into the new structure. The Widgets Bundle calls `modify_instance()` when it renders the widget, renders the form and saves the form, so the method must leave an instance that already has the new structure unchanged.

```php
function modify_instance( $instance ) {
	// Only apply the transformation if the instance does not already have the new structure.
	if ( empty( $instance['employee'] ) ) {
		$instance['employee'] = array();
		if ( isset( $instance['employee_name'] ) ) {
			$instance['employee']['name'] = $instance['employee_name'];
		}
		if ( isset( $instance['employee_surname'] ) ) {
			$instance['employee']['surname'] = $instance['employee_surname'];
		}

		unset( $instance['employee_name'] );
		unset( $instance['employee_surname'] );
	}
	return $instance;
}
```
