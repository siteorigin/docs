# Post Meta Box Forms

Occasionally a widget needs to be able to store and retrieve post specific data. We have made this possible using the `SiteOrigin_Widget_Meta_Box_Manager` class. It is a singleton which allows a widget to specify form fields in the familiar format used by SiteOrigin widgets to have their forms rendered. Get the instance with `SiteOrigin_Widget_Meta_Box_Manager::single()`. The `$sow_meta_box_manager` global isn't set until the `init` action, after widgets are initialized.

The fields appear in the **Widgets Bundle Post Meta Data** box in the Classic Editor. The Block Editor doesn't show this box.

## Adding Fields to the Widgets Bundle Post Meta Box
Fields are added using the `append_to_form()` function of the `SiteOrigin_Widget_Meta_Box_Manager` class. It takes the following parameters:
- $widget_id `string` Base id of the widget adding the fields.
- $fields `array` The fields to add.
- $post_types `string|array` _Optional_ A post type string, 'all' or an array of post types.

If more than one field is being added for a widget, it is encouraged to wrap the fields being added in a `section` field type to keep the form organized.

### Example - Adding Fields
The post meta box fields would typically be added in a widget's `initialize()` function.
```php
function initialize() {
    SiteOrigin_Widget_Meta_Box_Manager::single()->append_to_form(
        $this->id_base,
        array(
            'my_widget_fields_section' => array(
                'type' => 'section',
                'label' => __( 'My Widget Meta Fields', 'your-text-domain' ),
                'fields' => array(
                    'some_post_meta_text' => array(
                        'type' => 'text',
                        'label' => __( 'Meta Text', 'your-text-domain' )
                    ),
                )
            )
        )
    );
}
```

### Example - Adding Fields for a Specific Post Type
When an array of post types is specified as the third argument to the `append_to_form()` function, the form fields will only be displayed for that post type. 
```php
function initialize() {
    SiteOrigin_Widget_Meta_Box_Manager::single()->append_to_form(
        $this->id_base,
        array(
            'my_widget_fields_section' => array(
                'type' => 'section',
                'label' => __( 'My Widget Meta Fields', 'your-text-domain' ),
                'fields' => array(
                    'some_post_meta_text' => array(
                        'type' => 'text',
                        'label' => __( 'Meta Text', 'your-text-domain' )
                    ),
                )
            )
        ),
        array( 'post' )
    );
}
```

The `append_to_form()` function may be called multiple times by a widget and the additional fields for a widget will be appended and rendered. Caution must be taken not to append multiple fields with the same name and post types combination, or else the last appended form field will override any previously added ones. This can be avoided by ensuring name and post type combinations are unique for a widget.

## Retrieving Stored Post Meta Data
Once there is some post specific meta data stored, it may be retrieved by using the `get_widget_post_meta()` function of the `SiteOrigin_Widget_Meta_Box_Manager` class. It returns an empty string when nothing is stored. It takes the following parameters:
- $post_id `int` The id of the post for which the meta data is stored.
- $widget_id `string` The base id of the widget for which the meta data is stored.
- $meta_key `string` The key of the meta data value which is to be retrieved.

### Example - Retrieving Stored Post Meta Data
```php
$my_widget_post_meta = SiteOrigin_Widget_Meta_Box_Manager::single()->get_widget_post_meta(
    $post_id,
    $this->id_base,
    'my_widget_fields_section'
);
```