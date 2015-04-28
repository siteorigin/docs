# Post meta box forms
Occasionally a widget needs to be able to store and retrieve post specific data. We have made this possible using the `SiteOrigin_Meta_Box_Manager` class. It is a singleton which allows a widget to specify form fields in the familiar format used by SiteOrigin widgets to have their forms rendered.

## Adding fields to the Widgets Bundle post meta box
Fields are added using the `append_to_form()` function of the `SiteOrigin_Meta_Box_Manager` class. It takes the following parameters:
- $widget_id `string` Base id of the widget adding the fields.
- $fields `array` The fields to add.
- $post_types `string|array` _Optional_ A post type string, 'all' or an array of post types.

If more than one field is being added for a widget, it is encouraged to wrap the fields being added in a `section` field type to keep the form organized.

### Example - adding fields
The post meta box fields would typically be added in a widget's `initialize()` function.
```php
function initialize() {
    global $sow_meta_box_manager;
    $sow_meta_box_manager->append_to_form(
        $this->id_base,
        array(
            'my_widget_fields_section' => array(
                'type' => 'section',
                'label' => __( 'My Widget Meta Fields', 'siteorigin-widgets' ),
                'fields' => array(
                    'some_post_meta_text' => array(
                        'type' => 'text',
                        'label' => __( 'Meta Text', 'siteorigin-widgets' )
                    ),
                )
            )
        )
    );
}
```

### Example - adding fields for a specific post type
When an array of post types is specified as the third argument to the `append_to_form()` function, the form fields will only be displayed for that post type. 
```php
function initialize() {
    global $sow_meta_box_manager;
    $sow_meta_box_manager->append_to_form(
        $this->id_base,
        array(
            'my_widget_fields_section' => array(
                'type' => 'section',
                'label' => __( 'My Widget Meta Fields', 'siteorigin-widgets' ),
                'fields' => array(
                    'some_post_meta_text' => array(
                        'type' => 'text',
                        'label' => __( 'Meta Text', 'siteorigin-widgets' )
                    ),
                )
            )
        ),
        array( 'post' )
    );
}
```

The `append_to_form()` function may be called multiple times by a widget and the additional fields for a widget will be appended and rendered. Caution must be taken not to append multiple fields with the same name and post types combination, or else the last appended form field will override any previously added ones. This can be avoided by ensuring name and post type combinations are unique for a widget.

## Retrieving stored post meta data
Once there is some post specific meta data stored, it may be retrieved by using the `get_widget_post_meta()` function of the `SiteOrigin_Meta_Box_Manager` class. It takes the following parameters:
- $post_id `string` The id of the post for which the meta data is stored.
- $widget_id `string` The base id of the widget for which the meta data is stored.
- $meta_key `string` The key of the meta data value which is to be retrieved.

### Example - retrieving stored post meta data
```php
global $sow_meta_box_manager;
$my_widget_post_meta = $sow_meta_box_manager->get_widget_post_meta(
    $post_id,
    $this->id_base,
    'my_widget_fields_section'
);
```