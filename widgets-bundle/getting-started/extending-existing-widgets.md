# Extending existing widgets

When you're creating a theme or plugin, you might want to offer your users a custom version of one of the widgets that already comes with the SiteOrigin Widgets Bundle. Rather than create a new widget from scratch, you can just extend one of our widgets with your own custom style.

In this tutorial, we'll deal with adding a new style to the button widget.

## Modifying the form

The first thing you'll want to do is modify the default form to add in your own style. To get an idea of what the default form looks like, open the `so-widgets-bundle` directory and open up `widgets/so-button-widget/so-button-widget.php`. In the `__construct` function, you'll see that the 4th argument is an array that specifies the form. The button widget has a section called `design` and in that, a field called `theme`. 

The `theme` field is a method of allowing the user to select differnet button themes. We need to modify this field and add our own custom theme option, which will allow users to select our theme. Here's the PHP we would use to do that:

```php
function mytheme_extend_button_form( $form_options, $widget ){
	// Lets add a new theme option
	if( !empty($form_options['design']['fields']['theme']['options']) ) {
		$form_options['design']['fields']['theme']['options']['test'] = __('Test Style', 'mytheme');
	}

	return $form_options;
}
add_filter('siteorigin_widgets_form_options_sow-button', 'mytheme_extend_button_form', 10, 2);
```

Lets go over what's all happening here. First, we're creating a custom filter function called `mytheme_extend_button_form` and hooking it to `siteorigin_widget_form_options_sow-button`. This filter is run for every widget in the widget bundle and is of the form `siteorigin_widget_form_options_{$id_base}`, where `$id_base` is the first argument of the `__construct` argument we looked at earlier. In this case `sow-button`.

We're making sure that the option field we're looking for is there, and if it is, we're adding our own option called **Test Style**.

![Custom Button Theme](../images/custom-theme-field.png)

## Changing the template file

Now that we've added our own button theme to the `theme` drop down, we need to tell the SiteOrigin Button widget what template file to display when the user selects our theme from `theme` drop down what template file to use. In the SiteOrigin Widgets Bundle, a template is simply a PHP file. It gets passed the widget values in an `$instance` array, plus it has access to the values returned by `get_template_variables()`.

```php
function mytheme_button_template_file( $filename, $instance, $widget ){
	// Check if the user has selected your custom theme
	if( !empty($instance['design']['theme']) && $instance['design']['theme'] == 'test' ) {
		// This option works for plugins
		$filename = plugin_dir_path( __FILE__ ) . 'tpl/button.php';
		
		// And this one for themes
		$filename = get_stylesheet_directory() . '/tpl/button.php'; 
	}
	return $filename;
}
add_filter( 'siteorigin_widgets_template_file_sow-button', 'mytheme_button_template_file', 10, 3 );
```
The above code checks if the user has selected our button theme (that's what the if clause checks), and if it does, it loads the specified template. You'll want to change the `test` to the theme name you decided on and `button.php` to your template file.

To get an idea of what's available to a button widget, you can take a look at the one that comes with the which is `widgets/so-button-widget/tpl/base.php`. Modify this to your liking and put it in your theme or plugin. Read over the [HTML Templates](../templating/html-templates.md) section to find out more about creating templates.

Also, keep in mind that you can just skip specifying a custom template file, in which case the button widget will just use the default template file `base.php`.

## Changing the LESS file

We'll use a similar method to change the LESS file we use to generate the style for our custom button. Most of the time, this is going to be the only file you actually change.

```php
function mytheme_button_less_file( $filename, $instance, $widget ){
	// Check if the user has selected your custom theme
	if( !empty($instance['design']['theme']) && $instance['design']['theme'] == 'test' ) {
		$filename = plugin_dir_path( __FILE__ ) . 'less/test.less';
		
		// And this one for themes
		$filename = get_stylesheet_directory() . '/less/test.less'; 
	}
	return $filename;
}
add_filter( 'siteorigin_widgets_less_file_sow-button', 'mytheme_button_less_file', 10, 3 );
```

The LESS syntax is fairly simple, especially if you're just using it for variables and nexting. To learn more about using LESS in the Widgets Bundle, read over the [LESS Stylesheets](../templating/less-stylesheets.md) section of the docs.
