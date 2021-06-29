# Presets

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
