# Initializing a Widget

## The `initialize()` Method

A widget that extends `SiteOrigin_Widget` can override the `initialize()` method for its setup code. The code would also work in the widget's constructor, and `initialize()` keeps it separate and easier to read.

## Registering Front End Scripts and Styles

The `register_frontend_scripts()` and `register_frontend_styles()` methods of `SiteOrigin_Widget` load the scripts and styles that your template needs on the front end. The Widgets Bundle enqueues these files only on pages that show the widget. Each method takes an array of arrays, and each inner array holds the arguments you'd pass to [`wp_enqueue_script()`](https://developer.wordpress.org/reference/functions/wp_enqueue_script/) or [`wp_enqueue_style()`](https://developer.wordpress.org/reference/functions/wp_enqueue_style/). The second item is the file's URL, and a file path doesn't work there:

```php
function initialize() {
	$this->register_frontend_scripts(
		array(
			array( 'hello-world-script', plugin_dir_url( __FILE__ ) . 'js/script.js', array( 'jquery' ), '1.0' )
		)
	);
	
	$this->register_frontend_styles(
		array(
			array( 'hello-world-style', plugin_dir_url( __FILE__ ) . 'css/style.css', array(), '1.0' )
		)
	);
}
```
