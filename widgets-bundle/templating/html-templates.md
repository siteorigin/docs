# HTML Templates
HTML templates are used to render widgets for display in the front end. A template is included for output when the `widget()` function is called, so it has access to the widget `$instance` variable and the `$args` variable. The template's name must be specified by overriding the `get_template_name()` function. By default, it must be placed in a folder named _tpl_ in the root of the widget folder.

### Example - simple template
Overriding the `get_template_name()` function:
```php
function get_template_name( $instance ) {
    return 'my-awesome-template';
}
```

In the file found at tpl/my-awesome-template.php:
```php
<div>
    <?php echo $args['before_title'] ?>
    <h1><?php echo $instance['title'] ?></h1>
    <?php echo $args['after_title'] ?>
    <div>
        <a href="<?php $instance['link_url'] ?>"><?php $instance['link_text'] ?></a>
    </div>
</div>
```

## Template selection
A template may be selected at runtime based on an option specified by the user, found in the `$instance` argument passed in to the `get_template_name()` function.

### Example - template selection based on user input
```php
function get_template_name( $instance ) {
    $template_name = '';
    if( ! empty( $instance['use_blue_template'] ) ) {
        $template_name = 'blue_template';
    } else {
        $template_name = 'red_template';
    }
    return $template_name;
}
```

## Template variables
For convenience, before including the template file specified by `get_template_name()`, the `SiteOrigin_Widget` base class extracts variables in the array returned by the `get_template_variables()` function. This is useful when it's necessary to perform any transformations on values specified by the user.

### Example - using template variables
Return the variables for extraction in an array:
```php
function get_template_variables( $instance ) {
    return array(
        'title' => ! empty( $instance['title'] ) ? $instance['title'] : 'Default title',
        'link_url' => ! empty( $instance['link_url'] ) ? $instance['link_url'] : '',
        'link_text' => ! empty( $instance['link_text'] ) ? $instance['link_text'] : 'Default link text.',
    );
}
```

Use the extracted variable in a template:
```php
<div>
    <?php echo $args['before_title'] ?>
    <h1><?php echo $title ?></h1>
    <?php echo $args['after_title'] ?>
    <div>
        <a href="<?php $link_url ?>"><?php $link_text ?></a>
    </div>
</div>
```

## Escaping outputs
It is considered best practice to escape all potentially unsafe data as late as possible before outputting it to the front end. We strongly encourage this practice as it helps ensure security. The four built-in WordPress escaping functions (`esc_html`, `esc_url`, `esc_attr`, and `esc_js`) should be used as in the following example. More information on escaping data can be found <a href="http://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data#Escaping:_Securing_Output" target="_blank">here</a>.

### Example - escaping data before output
```php
<script type="text/javascript">
    var value = <?php esc_js( $js_value ) ?>;
</script>
<div>
    <?php echo esc_html( $args['before_title'] ) ?>
    <h1><?php echo esc_html( $title ) ?></h1>
    <?php echo esc_html( $args['after_title'] ) ?>
    <div class="<?php esc_attr( $style_attribute ) ?>">
        <a href="<?php esc_url( $link_url ) ?>"><?php esc_html( $link_text ) ?></a> 
    </div>
</div>
```