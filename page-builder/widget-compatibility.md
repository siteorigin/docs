# Making Your Widgets Page Builder Compatible

For the most part, Page Builder does a very good job of handling standard widgets. The only real issue comes in when a widget uses javascript that specifically targets the WordPress widget interface.

There are usually a few small changes you can make to to ensure your widget works in both the WordPress widgets interface and Page Builder.

### Loading Javascript

The first thing your widget needs to do is enqueue its CSS and Javascript for the Page Builder interface. This is usually as simple as hooking your existing enqueue function to `siteorigin_panel_enqueue_admin_scripts` action.

```php
/**
 * Enqueue all my widget's admin scripts
 */
function mywidget_enqueue_scripts() {
	wp_enqueue_script( ... );
}
add_action( 'admin_print_scripts-widgets.php', 'mywidget_enqueue_scripts' );
// Add this to enqueue your scripts on Page Builder too
add_action( 'siteorigin_panel_enqueue_admin_scripts', 'mywidget_enqueue_scripts' );

```

### Form Setup Actions

The next thing you need to do is ensure your widget form setup function is being called properly. You have 2 options here. Either you can include a `<script> ... </script>` in your widget form that sets up your widget form for both Page Builder and the WordPress widget interface.

The other option is to use the `panelsopen` jQuery event. This event is triggered right after the form HTML is loaded, so is a good opportuinity to set up your form.

```javascript
$( document ).on( 'panelsopen', function( e ) {
	var dialog = $( e.target );
	// Check that this is for our widget class
	if( !dialog.has( '.some-unique-widget-form-class' ) ) return;

	// Here we can setup our widget form.
} );
```

### Overriding Automatic Widget Description

By default, Page Builder will override a widgets description to allow users to more easily identify widgets without needing to open them. Page Builder will look for the first valid non-empty input (with a preference for fields called "title" and "text") and use that for its description. This can, however, be problematic for certain fields so you may wish to tell Page Builder to look for a specific field, or completely disable the automatic widget description altogether. This is done by adjusting the `panels_title` option in the widget's `$widget_options` parameter when constructing the widget.

You can disable the widget descriptions by setting `panels_title` to `false`.

```php
function __construct() {
		parent::__construct(
			'sow-demo-widget',
			__( 'SiteOrigin Demo Widget', 'siteorigin-docs' ),
			array(
				'panels_title' => 'false', // Disable Widget description override.
			),
			array(),
			false,
			plugin_dir_path( __FILE__ )
		);
	}
```

You can tell Page Builder to find a field with a specific name by setting `panels_title` to the field you would like for it to use. For example, if you have a field called "username" you would use:

```php
function __construct() {
		parent::__construct(
			'sow-demo-widget',
			__( 'SiteOrigin Demo Widget', 'siteorigin-docs' ),
			array(
				'description' => __( "This will only be used if a username isn't set.", 'siteorigin-docs' ),
				'panels_title' => 'username', // Tell Page Builder to look for the "username" field"
			),
			array(),
			false,
			plugin_dir_path( __FILE__ )
		);
	}
```

If Page Builder isn't able to find a valid field with that title, it'll fallback to the standard widget description.
