# Presets

The presets field gives users a list of ready-made settings for your widget. When a user chooses a preset, the field copies the preset's values into the widget's form.

## Building the Presets Array

Each preset is an item in an array. The item's key is the preset's slug, which identifies the preset in the presets list. The item holds a `label`, the name users see in the list, and the preset's `values`:

```php
$presets = array(
	'preset-slug' => array( // Slug.
		'label' => 'Visible preset name', // Preset name.
		'values' => array(),
	),
);
```

Each key in `values` must match a field in your form. A section takes a nested associative array, and a repeater takes an indexed array of items:

```php
'values' => array(
	'setting' => 'example',
	'setting-2' => 'value',
	'design' => array( // Section name.
		'header-background' => '#0f0', // Setting inside of a section.
		'setting' => "Setting names don't have to be unique",
	),
),
```

A preset only needs values for the fields it changes, and every other field keeps its value. Add more presets as more items in the array:

```php
$presets = array(
	'preset-slug' => array( // Slug
		'label' => 'Visible preset name', // Preset name
		'values' => array(
			'setting' => 'example',
			'setting-2' => 'value',
			'design' => array( // Section name.
				'header-background' => '#0f0', // Setting inside of a section.
				'setting' => "Setting names don't have to be unique",
			),
		),
	),
	'another-example' => array( // Slug.
		'label' => 'Example 2', // Preset name.
		'values' => array(
			'setting' => 'This is',
			'setting-2' => 'Another Example',
			'design' => array( // Section name.
				'header-background' => '#000', // Setting inside of a section.
				'setting' => "Test",
			),
		),
	),
);
```

## Adding the Field

Pass your presets array to a `presets` field in its `options` argument. The optional `default_preset` argument takes a preset's slug, and the list then has no empty option:

```php
'preset' => array(
	'type' => 'presets',
	'label' => __( 'Preset', 'siteorigin-docs' ),
	'options' => $presets,
	'default_preset' => 'preset-slug',
),
```

When a user chooses a preset, the field copies the preset's values into the form and shows an **Undo** link that restores the previous values. The field shows a warning as its description, and a `description` argument replaces the warning.

## Storing Presets in a JSON File

Presets are easier to manage in a JSON file. JSON is stricter than PHP arrays, so check the file with a JSON validator if your presets don't load. This example is a `presets.json` file in a `data` folder in your widget folder:

```json
{
	"test": {
		"label": "Test 1",
		"values": {
			"test": "Test 1 was selected."
		}
	},
	"test-2": {
		"label": "Test 2",
		"values": {
			"test": "Test 1 wasn't selected. Test 2 was selected instead.",
			"color": "#0f0"
		}
	}
}
```

This PHP loads the file:

```php
$presets = json_decode( file_get_contents( plugin_dir_path( __FILE__ ) . 'data/presets.json' ), true );
```

## Showing Fields for Each Preset

The presets field works with [state emitters](state-emitters.md), so the form shows only the fields that the selected preset sets. Setting up a state handler for every field by hand is error-prone with many presets, so `SiteOrigin_Widget` has a `dynamic_preset_state_handler()` method that adds the handlers from your preset data. The method returns your fields array with the handlers added, and it takes three arguments:

- `$state_name` (`string`): the name of the state, which you set in the presets field's `state_emitter`.
- `$preset_data` (`array`): your presets.
- `$fields` (`array`): the fields to add state handlers to.

The method adds a handler only to fields inside a `section`, including nested sections. Each of those fields shows while a preset that sets the field's value is selected, and hides for every other preset. Top-level fields, top-level sections and fields that already have a `state_handler` keep their settings.

### Example

```php
public function get_widget_form() {
	$presets = array(
		'test' => array( // Preset 1.
			'label' => 'Test 1',
			'values' => array(
				'settings' => array(
					'test' => 'Test 1 example text',
				),
			),
		),
		'test-2' => array( // Preset 2.
			'label' => 'Test 2',
			'values' => array(
				'settings' => array(
					'test' => 'Test 2 example text',
					'color' => '#0f0',
				),
			),
		),
	);

	return $this->dynamic_preset_state_handler(
		'selected_theme', // state_name
		$presets, // preset_data
		array( // fields
			'preset' => array(
				'type' => 'presets',
				'label' => __( 'Theme', 'siteorigin-docs' ),
				'options' => $presets,
				'state_emitter' => array(
					'callback' => 'select',
					'args' => array( 'selected_theme' ), // state_name
				),
			),
			'settings' => array(
				'type' => 'section',
				'label' => __( 'Settings', 'siteorigin-docs' ),
				'fields' => array(
					'test' => array(
						'type' => 'text',
						'label' => __( 'Text', 'siteorigin-docs' ),
					),
					'color' => array(
						'type' => 'color',
						'label' => __( 'Color', 'siteorigin-docs' ),
					),
				),
			),
		)
	);
}
```

In this example, the **Text** field shows for both presets, and the **Color** field shows only when **Test 2** is selected.

## Test Plugin

The [test plugin](https://siteorigin.com/wp-content/uploads/2021/06/siteorigin-preset-field-demo.zip) shows the presets field in a working widget:

1. Download the plugin.
2. Go to **Plugins > Add Plugin**, click **Upload Plugin** and upload `siteorigin-preset-field-demo.zip`.
3. Activate the **SiteOrigin - Preset Field** plugin.
4. Go to **Plugins > SiteOrigin Widgets** and activate the **SiteOrigin Preset Field** widget.
5. Add the **SiteOrigin Preset Field** widget to a page in Page Builder.
