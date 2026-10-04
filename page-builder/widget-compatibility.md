# Making Your Widgets Page Builder Compatible

Standard widgets work in Page Builder without changes. A widget needs a few changes if its JavaScript targets the WordPress Widgets screen, because Page Builder loads widget forms in its own dialog.

## Loading Your Scripts

Load your widget's admin CSS and JavaScript in Page Builder as well as on the Widgets screen. Hook your existing enqueue function to the `siteorigin_panel_enqueue_admin_scripts` action:

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

## Setting Up the Form

Your widget's form setup code needs to run when Page Builder opens the form. Either add a `<script>` to your widget form that sets up the form, which runs on the Widgets screen and in Page Builder, or listen for the `panelsopen` jQuery event, which Page Builder triggers right after it loads the form HTML:

```javascript
( function( $ ) {
	$( document ).on( 'panelsopen', function( e ) {
		var dialog = $( e.target );
		// Check that this is for our widget class
		if ( ! dialog.find( '.some-unique-widget-form-class' ).length ) {
			return;
		}

		// Here we can setup our widget form.
	} );
} )( jQuery );
```

## Choosing the Widget Summary

In the builder, Page Builder shows a summary under each widget's title, so users can tell widgets apart without opening them. The summary comes from the widget's first non-empty field, with fields named "title" and "text" first. To use a different field, or to show the widget's description instead, set the `panels_title` option in the widget's `$widget_options`. The examples below extend `SiteOrigin_Widget`. A `WP_Widget` takes the same option in its third argument, `$widget_options`.

Setting `panels_title` to `false` shows the widget's description:

```php
function __construct() {
	parent::__construct(
		'sow-demo-widget',
		__( 'SiteOrigin Demo Widget', 'siteorigin-docs' ),
		array(
			'panels_title' => false, // Disable Widget description override.
		),
		array(),
		false,
		plugin_dir_path( __FILE__ )
	);
}
```

Setting `panels_title` to a field's name uses that field. This example uses a field named "username":

```php
function __construct() {
	parent::__construct(
		'sow-demo-widget',
		__( 'SiteOrigin Demo Widget', 'siteorigin-docs' ),
		array(
			'description'  => __( "This will only be used if a username isn't set.", 'siteorigin-docs' ),
			'panels_title' => 'username', // Tell Page Builder to look for the "username" field"
		),
		array(),
		false,
		plugin_dir_path( __FILE__ )
	);
}
```

If the field is empty or missing, Page Builder shows the widget's description. Page Builder looks for the `panels_title` field inside sections and repeaters only when you also set `panels_title_check_sub_fields` to `true`.
