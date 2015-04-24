# Initializing a widget

## The `initialize()` method
A widget extending `SiteOrigin_Widget` can optionally override the `initialize` method to handle any required initialization steps. This code could technically be put in the widget constructor, but we use this method for better code organization and readability.

## Registering front end scripts and styles
The `SiteOrigin_Widget` base class provides two convenience methods, `register_frontend_scripts` and `register_frontend_styles`, for enqueueing scripts and styles necessary for rendering the template on the front end. The arguments to these methods are simply an array of arrays. Each array contains the arguments exactly as they would appear when calling [`wp_enqueue_script`](https://codex.wordpress.org/Function_Reference/wp_enqueue_script) or [`wp_enqueue_style`](https://codex.wordpress.org/Function_Reference/wp_enqueue_style).

```php
function initialize() {
    $this->register_frontend_scripts(
        array(
            array( 'hello-world-script', 'absolute/path/to/my/script.js, array( 'jquery' ), '1.0' )
        )
    );
    
    $this->register_frontend_styles(
        array(
            array( 'hello-world-style', 'absolute/path/to/my/style.css, array(), '1.0' )
        )
    );
}
```
