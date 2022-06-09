# Template file filter

This filter gives you a way to change which file is being used to render/display your widget. By default, Widgets Bundle will look for a file in your template directory called `default.php` in the `tpl` directory and this filter will allow you to change it to something else.

This filter perfect for conditional templates based on settings and as an example of that we've written a guide called [extending existing widgets](../../getting-started/extending-existing-widgets.md) that outlines this usecase.

This filter has the form `'siteorigin_widgets_template_file_' . $this->id_base`, where id_base is the ID of the widget.

```php
function mytheme_button_template_file( $filename, $instance, $widget ){
    if ( ! empty( $instance['design']['theme'] ) && $instance['design']['theme'] == 'test' ) {
        // This option works for plugins
        $filename = plugin_dir_path( __FILE__ ) . 'tpl/button.php';
        
        // And this one for themes
        $filename = get_stylesheet_dir() . '/tpl/button.php'; 
    }
    return $filename;
}
add_filter( 'siteorigin_widgets_template_file_sow-button', 'mytheme_button_template_file', 10, 3 );
```
