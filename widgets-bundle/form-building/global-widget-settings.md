# Global Widget Settings

Global settings apply to every copy of a widget on the site. Add a `get_settings_form()` method to your widget that returns a standard form array, as described in [Form Fields](./form-fields.md), and users find the settings at **Plugins > SiteOrigin Widgets** under the widget's **Settings** button.

## Example

This example adds a checkbox to the widget's global settings:

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here.

	public function get_settings_form() {
		return array(
			'example' => array(
				'type' => 'checkbox',
				'label' => __( 'Example', 'siteorigin-docs' ),
			),
		);
	}
}
```

## Getting the Settings

The `get_global_settings()` method of `SiteOrigin_Widget` returns the widget's global settings. This example gets every setting and checks the example checkbox:

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here.

	public function modify_instance( $instance ) {

		// Retrieve all settings.
		$global_settings = $this->get_global_settings();

		if ( ! empty( $global_settings['example'] ) ) {
			// Global example setting is enabled. Do something here.
		}

		return $instance;
	}
}
```

Pass a setting's name to `get_global_settings()` to get that setting only:

```php
class MyCustomWidget extends SiteOrigin_Widget {
	// We're leaving out all the setup code here.

	public function modify_instance( $instance ) {

		if ( ! empty( $this->get_global_settings( 'example' ) ) ) {
			// Global example setting is enabled. Do something here.
		}

		return $instance;
	}
}
```

## Adding Settings to Other Widgets

The `siteorigin_widgets_settings_form` filter changes the global settings of every widget. To change one widget's settings, add its `id_base` to the filter name, such as `siteorigin_widgets_settings_form_sow-button` for the Button Widget. Both filters also pass the widget object as a second argument. This example adds a checkbox to the Button Widget's global settings:

```php
add_filter( 'siteorigin_widgets_settings_form_sow-button', function( $form_options ) {
	$form_options['example'] = array(
		'type' => 'checkbox',
		'label' => __( 'Example', 'siteorigin-docs' ),
	);

	return $form_options;
} );
```

![Widget Form Text Input](../images/form-building-global-widget-settings-button.png)
