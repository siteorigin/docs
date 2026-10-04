# HTML Templates

An HTML template outputs a widget on the front end. The Widgets Bundle includes the template when it calls the widget's `widget()` method, so the template can use the widget's `$instance` and `$args` variables. The Widgets Bundle looks for `tpl/default.php` in your widget folder.

Override `get_template_name()` to use another template, and keep the template in the `tpl` folder at the root of the widget folder.

### Example: Naming the Template

Override `get_template_name()`:

```php
function get_template_name( $instance ) {
	return 'my-awesome-template';
}
```

Then add the template, `tpl/my-awesome-template.php`:

```php
<div>
	<?php echo $args['before_title'] . esc_html( $instance['title'] ) . $args['after_title']; ?>
	<div>
		<a href="<?php echo esc_url( $instance['link_url'] ); ?>"><?php echo esc_html( $instance['link_text'] ); ?></a>
	</div>
</div>
```

## Choosing a Template From a Setting

`get_template_name()` receives the widget's `$instance`, so it can return a different template for each value of a setting.

### Example: Choosing a Template

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

Before the Widgets Bundle includes the template, `SiteOrigin_Widget` extracts the array that `get_template_variables()` returns into variables, so the template uses values you've already prepared, such as defaults for empty fields.

### Example: Using Template Variables

Return the variables in an array:

```php
function get_template_variables( $instance, $args ) {
	return array(
		'title' => ! empty( $instance['title'] ) ? $instance['title'] : 'Default title',
		'link_url' => ! empty( $instance['link_url'] ) ? $instance['link_url'] : '',
		'link_text' => ! empty( $instance['link_text'] ) ? $instance['link_text'] : 'Default link text.',
	);
}
```

Then use the variables in the template:

```php
<div>
	<?php echo $args['before_title'] . esc_html( $title ) . $args['after_title']; ?>
	<div>
		<a href="<?php echo esc_url( $link_url ); ?>"><?php echo esc_html( $link_text ); ?></a>
	</div>
</div>
```

## Escaping Output

Escape every value as late as possible, just before you output it, with the WordPress escaping functions, such as `esc_html()`, `esc_url()`, `esc_attr()` and `wp_json_encode()`. The `before_title` and `after_title` values in `$args` hold HTML from the theme's widget area, so output them without escaping. [Escaping Data](https://developer.wordpress.org/apis/security/escaping/) on WordPress.org has the details.

### Example: Escaping Values

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
