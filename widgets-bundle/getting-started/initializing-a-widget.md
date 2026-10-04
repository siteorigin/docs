# Initializing a Widget

## The `initialize()` Method
A widget extending `SiteOrigin_Widget` can optionally override the `initialize` method to handle any required initialization steps. This code could technically be put in the widget constructor, but we use this method for better code organization and readability.

## Registering Front End Scripts and Styles
The `SiteOrigin_Widget` base class provides two convenience methods, `register_frontend_scripts` and `register_frontend_styles`, for enqueueing scripts and styles necessary for rendering the template on the front end. The arguments to these methods are simply an array of arrays. Each array contains the arguments exactly as they would appear when calling [`wp_enqueue_script`](https://developer.wordpress.org/reference/functions/wp_enqueue_script/) or [`wp_enqueue_style`](https://developer.wordpress.org/reference/functions/wp_enqueue_style/). The second item is the file's URL, not a file path. The Widgets Bundle enqueues these files only when the widget is displayed.

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
