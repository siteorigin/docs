# Presets

The Preset form field allows you to create presets for your widget. To do this you'll need to create a multidimensional array containing your setting selections.

The first level of the array is where the slug of the preset is stored. This slug is used to uniquely identify the preset as a select option value.

The second level contains the label, which is the visible text used in the presets drop down, and the preset values.

```php
$presets = array(
    'preset-slug' => array( // Slug
        'label' => 'Visible preset name', // Preset name
        'values' => array(),
    ),
);
```

Each element in the values array must correspond with a valid setting. Selectors and repeaters are supported through nested arrays.

```php
'values' => array(
    'setting' => 'example',
    'setting-2' => 'value',
    'design' => array( // Section name
        'header-background' => '#0f0', // Setting inside of a section.
        'setting' => "Setting names don't have to be unique",
    ),
),
```

Your preset does not have to adjust all of your fields. Any fields not adjusted will retain their original values.

Now that you've structured your first preset, you can add additional presets to the first level array by creating another element in the first level of the presets array.

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
    'another-example' => array( // Slug
        'label' => 'Example 2', // Preset name
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

### Storing Presets as JSON and Loading Presets

To make managing presets easier, we recommend loading your presets from a json file. How you structure json is much stricter when compared to standard PHP so I recommend using a JSON validator if you're having trouble getting your preset JSON to load.

The following snippet is the contents of presets.json which can be added to a data directory in your widgets directory.

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

The following PHP will allow you to load the presets.json file in your widgets data directory:

```php
$presets = json_decode( file_get_contents( plugin_dir_path( __FILE__ ) . 'data/presets.json' ), true );
```

### Dynamic State Handler

The preset field is able to work in combination with the [State Emitters](state-emitters.md). Due to complicated nature of manging state_handlers when there's a number of presets present, we've created a utility method for widgets called `dynamic_preset_state_handler`. This method allows you to automatically add `state_handlers` based on the data in your presets. This method has three required parameters:

- state_name: `string` The name of the state. This is set when creating the `state_emitter`.
- preset_data: `array` An array containing your preset data.
- fields: `array` An array containing the fields you want to add a `state_handler` to.

#### Example

```php
$presets = array(
    'test' => array( // Preset 1
        'label' => 'Test 1',
        'values' => array(
            'test' => 'Test 1 example text',
        ),
    ),
    'test-2' => array( // Preset 2
        'label' => 'Test 2',
        'values' => array(
            'test' => 'Test 1 example text',
            'color' => '#0f0',
        ),
    ),
);

$this->dynamic_preset_state_handler(
    'selected_theme', // state_name
    $presets, // preset_data
    array( // fields
        'preset' => array(
            'type' => 'presets',
            'label' => __( 'Theme', 'siteorigin-docs'),
            'options' => $presets,
            'state_emitter' => array(
                'callback' => 'select',
                'args' => array( 'selected_theme' ),
            ),
        ),
        'test' => array(
            'type' => 'text',
            'label' => __( 'Text', 'siteorigin-docs' ),
        ),
        'color' => array(
            'type' => 'color',
            'label' => __( 'color', 'siteorigin-docs' ),
        ),
    )
);
```

### Test Plugin

Due to the complicated nature of this, we've prepared you a test plugin for you to try this field. You can [download it by clicking here](https://siteorigin.com/wp-content/uploads/2021/06/siteorigin-preset-field-demo.zip). Once downloaded, please navigate to **Plugins > Add New** and upload **siteorigin-preset-field-demo.zip**. When prompted, activate the **SiteOrigin - Preset Field** plugin.

Once installed, navigate to **Plugins > SiteOrigin Widgets** and activate the **SiteOrigin Preset Field** widget. Open any Page Builder powered page and add the **SiteOrigin Preset Field** widget to your page.
