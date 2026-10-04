# Adding a Custom Widget to Your Theme

Your theme can ship its own widgets, which users get when they install the Widgets Bundle. To change one of our existing widgets, follow [Extending Existing Widgets](../getting-started/extending-existing-widgets.md).

## Creating a Widgets Folder

Create a folder in your theme for your widgets, such as `widgets`, and register it as a Widgets Bundle folder. Add this function to your theme's `functions.php` file:

```php
function wbexample_add_widget_folders( $folders ) {
	$folders[] = get_template_directory() . '/widgets/';
	return $folders;
}
add_filter( 'siteorigin_widgets_widget_folders', 'wbexample_add_widget_folders' );
```

The function tells the Widgets Bundle to look for widgets in the `widgets` folder of the theme.

## Adding a Widget to the Folder

This example builds a staff widget. Create a `simple-staff-widget` folder with a `simple-staff-widget.php` file in it, and add `tpl`, `styles` and `assets` folders as your widget needs them.

![](./images/theme-widget-folder.png)

[Creating a Widget](../getting-started/creating-a-widget.md), [HTML Templates](../templating/html-templates.md) and [LESS Stylesheets](../templating/less-stylesheets.md) explain each part of the widget.

## Activating Your Widget

New widgets start inactive. Users can activate your widget at **Plugins > SiteOrigin Widgets**, or your theme can activate it with the `activate_widget()` method of `SiteOrigin_Widgets_Bundle`:

```php
function wbexample_activate_bundled_widgets() {
	// Stop if the Widgets Bundle isn't active.
	if ( ! class_exists( 'SiteOrigin_Widgets_Bundle' ) ) {
		return;
	}

	if ( ! get_theme_mod( 'bundled_widgets_activated' ) ) {
		if ( SiteOrigin_Widgets_Bundle::single()->activate_widget( 'simple-staff-widget' ) ) {
			set_theme_mod( 'bundled_widgets_activated', true );
		}
	}
}
add_action( 'admin_init', 'wbexample_activate_bundled_widgets' );
```

This function activates the widget in the `simple-staff-widget` folder on `admin_init`. `activate_widget()` takes the widget's folder name as its ID. A theme mod records the activation, so the function stops running once the widget is active.
