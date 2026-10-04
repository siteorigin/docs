# HTML Templates

HTML templates are used to render widgets for display on the front end. A template is included for output when the `widget()` function is called, so it has access to the widget `$instance` variable and the `$args` variable. By default, Widgets Bundle will attempt to use `tpl/default.php` in your widget directory.

The template's name can be optionally overridden by using the `get_template_name()` function. By default, it must be placed in a folder named _tpl_ in the root of the widget folder.

### Example - Simple Template
Overriding the `get_template_name()` function:
```php
function get_template_name( $instance ) {
	return 'my-awesome-template';
}
```

In the file found at tpl/my-awesome-template.php:
```php
<div>
	<?php echo $args['before_title'] . esc_html( $instance['title'] ) . $args['after_title']; ?>
	<div>
		<a href="<?php echo esc_url( $instance['link_url'] ); ?>"><?php echo esc_html( $instance['link_text'] ); ?></a>
	</div>
</div>
```

## Template Selection
A template may be selected at runtime based on an option specified by the user, found in the `$instance` argument passed in to the `get_template_name()` function.

### Example - Template Selection Based on User Input
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

## Template Variables
For convenience, before including the template file specified by `get_template_name()`, the `SiteOrigin_Widget` base class extracts variables in the array returned by the `get_template_variables()` function. This is useful when it's necessary to perform any transformations on values specified by the user.

### Example - Using Template Variables
Return the variables for extraction in an array:
```php
function get_template_variables( $instance, $args ) {
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
	<?php echo $args['before_title'] . esc_html( $title ) . $args['after_title']; ?>
	<div>
		<a href="<?php echo esc_url( $link_url ); ?>"><?php echo esc_html( $link_text ); ?></a>
	</div>
</div>
```

## Escaping Outputs
It is considered best practice to escape all potentially unsafe data as late as possible before outputting it to the front end. We strongly encourage this practice as it helps ensure security. Use the WordPress escaping functions, such as `esc_html()`, `esc_url()`, `esc_attr()` and `wp_json_encode()`, as in the following example. The `before_title` and `after_title` values in `$args` hold HTML from the theme's sidebar, so they're output without escaping. More information on escaping data can be found <a href="https://developer.wordpress.org/apis/security/escaping/" target="_blank">here</a>.

### Example - Escaping Data Before Output
```php
<script type="text/javascript">
	var value = <?php echo wp_json_encode( $js_value ); ?>;
</script>
<div>
	<?php echo $args['before_title'] . esc_html( $title ) . $args['after_title']; ?>
	<div class="<?php echo esc_attr( $style_attribute ); ?>">
		<a href="<?php echo esc_url( $link_url ); ?>"><?php echo esc_html( $link_text ); ?></a>
	</div>
</div>
```
