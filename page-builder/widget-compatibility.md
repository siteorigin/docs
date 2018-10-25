# Making Your Widgets Page Builder Compatible

For the most part, Page Builder does a very good job of handling standard widgets. The only real issue comes in when a widget uses javascript that specifically targets the WordPress widget interface.

There are usually a few small changes you can make to to ensure your widget works in both the WordPress widgets interface and Page Builder.

### Loading Javascript

The first thing your widget needs to do is enqueue its CSS and Javascript for the Page Builder interface. This is usually as simple as hooking your existing enqueue function to `siteorigin_panel_enqueue_admin_scripts` action.

```php
/**
 * Enqueue all my widget's admin scripts
 */
function mywidget_enqueue_scripts(){
	wp_enqueue_script( ... );
}
add_action('admin_print_scripts-widgets.php', 'mywidget_enqueue_scripts');
// Add this to enqueue your scripts on Page Builder too
add_action('siteorigin_panel_enqueue_admin_scripts', 'mywidget_enqueue_scripts');

```

### Form Setup Actions

The next thing you need to do is ensure your widget form setup function is being called properly. You have 2 options here. Either you can include a `<script> ... </script>` in your widget form that sets up your widget form for both Page Builder and the WordPress widget interface.

The other option is to use the `panelsopen` jQuery event. This event is triggered right after the form HTML is loaded, so is a good opportuinity to set up your form.

```javascript
$(document).on('panelsopen', function(e) {
	var dialog = $(e.target);
	// Check that this is for our widget class
	if( !dialog.has('.some-unique-widget-form-class') ) return;

	// Here we can setup our widget form.
});
```
