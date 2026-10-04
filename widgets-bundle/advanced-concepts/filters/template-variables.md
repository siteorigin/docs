# Template Variables Filter

The Widgets Bundle passes the widget's instance to `get_template_variables()`, which your widget overrides to return the variables for its template. The Widgets Bundle runs the result through the `siteorigin_widgets_template_variables_{$id_base}` filter, where `{$id_base}` is the widget's base ID, and then through `extract()`:

```php
$template_vars = $this->get_template_variables($instance, $args);
$template_vars = apply_filters( 'siteorigin_widgets_template_variables_' . $this->id_base, $template_vars, $instance, $args, $this );
extract( $template_vars );
```

Use the filter to change the template variables of another widget. Changing the front-end instance would also work, and this filter leaves the instance that the LESS stylesheet uses unchanged. The filter passes four arguments:

```php
/**
 * @param array $template_vars The Template variables array
 * @param array $instance The widget instance
 * @param array $args The widget display arguments, such as before_title and after_title
 * @param SiteOrigin_Widget $widget The main widget instance.
 */
function wbe_filter_template_variables( $template_vars, $instance, $args, $widget ){
    // Strictly this isn't correct, but we'll use it as an example
    $template_vars['text'] = __( $instance['text'], 'test-domain' );
    return $template_vars;
}
add_filter( 'siteorigin_widgets_template_variables_sow-button', 'wbe_filter_template_variables', 10, 4 );
```
