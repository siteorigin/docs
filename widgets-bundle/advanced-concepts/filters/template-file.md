# Template file filter

This filter gives you a way to change which file is being used to render/display your widget. We use this filter in our section on [extending existing widgets](../../getting-started/extending-existing-widgets.md). This guide will give you the best idea of what this filter is for and how you'll likely be using it.

This filter has the form `'siteorigin_widgets_template_file_' . $this->id_base`, where id_base is the ID of the widget.

```php
function mytheme_button_template_file( $filename, $instance, $widget ){
    if( !empty($instance['design']['theme']) && $instance['design']['theme'] == 'test' ) {
        // This option works for plugins
        $filename = plugin_dir_path( __FILE__ ) . 'tpl/button.php';
        
        // And this one for themes
        $filename = get_stylesheet_dir() . '/tpl/button.php'; 
    }
    return $filename;
}
add_filter( 'siteorigin_widget_template_file_sow-button', 'mytheme_button_template_file', 10, 3 );
```