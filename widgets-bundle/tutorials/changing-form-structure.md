# Changing Form Structure

Creating a widget is an iterative process. A form structure that might have made sense at one stage might need to be improved as your widget evolves. To handle these cases the `SiteOrigin_Widget` class provides the `modify_instance()` method, which you can override to transform a widget instance from the old structure to the new one.

## Updating form_options
Let's say we want to clean up a widget's form options by grouping a few together in a section. The first thing to do is update the array returned by `get_widget_form()`. For example:

Old form options:
```php
$form_options = array(
    'employee_name' => array(
        'type' => 'text',
        'label' => __( 'Name', 'example-text-domain' )
    ),
    'employee_surname' => array(
        'type' => 'text',
        'label' => __( 'Surname', 'example-text-domain' )
    ),
);
```

New form options:
```php
$form_options = array(
    'employee' => array(
        'type' => 'section',
        'label' => __( 'Employee', 'example-text-domain' ),
        'fields' => array(
            'name' => array(
                'type' => 'text',
                'label' => __( 'Name', 'example-text-domain' )
            ),
            'surname' => array(
                'type' => 'text',
                'label' => __( 'Surname', 'example-text-domain' )
            ),
        )
    )
);
```

## Overriding `modify_instance()`
Now to transform a widget instance's structure we override the `modify_instance()` method and apply the desired changes as follows. The Widgets Bundle calls `modify_instance()` when it displays the widget, renders the form and saves the form, so the method must leave an instance that already has the new structure unchanged.

```php
function modify_instance( $instance )  {
    // Only apply the transformation if the instance does not already have the new structure.
    if( empty( $instance['employee'] ) ) {
        $instance['employee'] = array();
        if( isset( $instance['employee_name'] ) ) $instance['employee']['name'] = $instance['employee_name'];
        if( isset( $instance['employee_surname'] ) ) $instance['employee']['surname'] = $instance['employee_surname'];
        
        unset( $instance['employee_name'] );
        unset( $instance['employee_surname'] );
    }
    return $instance;
}
```