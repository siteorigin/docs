# Post Meta Box Forms

The `SiteOrigin_Widget_Meta_Box_Manager` class lets a widget save data for each post, with form fields in the same format as a widget form. The class is a singleton, so get its instance with `SiteOrigin_Widget_Meta_Box_Manager::single()`. The `$sow_meta_box_manager` global isn't set until the `init` action, after widgets initialize.

The fields appear in the **Widgets Bundle Post Meta Data** box in the Classic Editor. The Block Editor doesn't show the box.

## Adding Fields

The `append_to_form()` method of `SiteOrigin_Widget_Meta_Box_Manager` adds fields to the box. It takes these arguments:

- `$widget_id` (`string`): the base ID of the widget that adds the fields.
- `$fields` (`array`): the fields to add.
- `$post_types` (`string|array`, optional): a post type, `'all'` or an array of post types.

If your widget adds several fields, wrap them in a `section` field to keep the box organized.

### Example: Adding Fields

Add the fields in your widget's `initialize()` method:

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

### Example: Adding Fields to One Post Type

An array of post types in the third argument shows the fields only on those post types:

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

A widget can call `append_to_form()` several times, and the box shows every field it adds. If two fields share a name and a post type, the field added last replaces the earlier field, so give each field a unique name for each post type.

## Getting the Saved Data

The `get_widget_post_meta()` method of `SiteOrigin_Widget_Meta_Box_Manager` returns a post's saved data, or an empty string when nothing is saved. It takes these arguments:

- `$post_id` (`int`): the ID of the post.
- `$widget_id` (`string`): the base ID of the widget that saved the data.
- `$meta_key` (`string`): the key of the value to get.

### Example: Getting Saved Data

```php
$my_widget_post_meta = SiteOrigin_Widget_Meta_Box_Manager::single()->get_widget_post_meta(
    $post_id,
    $this->id_base,
    'my_widget_fields_section'
);
```
