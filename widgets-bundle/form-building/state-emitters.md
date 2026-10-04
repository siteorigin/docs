# Modifying Forms With State Emitters

State emitters show, hide or change parts of a widget's form as the user fills it in. A field with a state emitter sends a state to the rest of the form when its value changes, and fields with state handlers act on that state. Build a few forms before you add state emitters, because they add a layer of logic on top of the form array.

## Form States

A state belongs to one widget form, and the form holds one state per group. The string `group[state]` names a state, and group and state names can contain letters, numbers, underscores and hyphens.

## State Emitters

A state emitter belongs to a field, and it sends states based on the field's value and the emitter's arguments. Add it to the field in the form array:

```php
'map_type'    => array(
    'type'    => 'radio',
    'default' => 'interactive',
    'label'   => __( 'Map type', 'siteorigin-docs' ),
    'state_emitter' => array(
        'callback' => 'select',
        'args' => array( 'map_type' )
    ),
    'options' => array(
        'interactive' => __( 'Interactive', 'siteorigin-docs' ),
        'static'      => __( 'Static image', 'siteorigin-docs' ),
    )
),
```

Each time this field changes, the form takes on the states that the emitter's callback returns. To give one field several emitters, set `state_emitter` to an array of emitter arrays. The Widgets Bundle has three built-in callbacks.

### The `select` Callback

The `select` callback sets each group in `args` to the field's value. Use it with a select or radio field.

### The `in` Callback

The `in` callback checks whether the field's value is in a list, so you can group several options into one state:

```php
'state_emitter' => array(
    'callback' => 'in',
    'args' => array(
        'group[state_1]: option1, option2',
        'group[state_2]: option3, option4',
    )
),
```

### The `conditional` Callback

The `conditional` callback evaluates a JavaScript expression that returns a boolean, where `val` holds the field's value:

```php
'state_emitter' => array(
    'callback' => 'conditional',
    'args' => array(
        'group[state_0]: val == 50',
        'group[state_1]: val > 50',
        'group[state_2]: val < 50',
    )
),
```

### Custom Callbacks

Add your own callback in JavaScript as a function on the global `sowEmitters` object. The Widgets Bundle creates `sowEmitters` in its `siteorigin-widget-admin` script, so enqueue your script from your widget's `enqueue_admin_scripts()` method with `siteorigin-widget-admin` as a dependency. The Widgets Bundle ignores callback names that start with an underscore.

```javascript
sowEmitters.custom = function( val, args, field ){
    var returnStates = {};
    
    // Use the field value in val and the args to set the group states
    returnStates['group'] = 'state';
    
    return returnStates;
};
```

The `field` argument is the jQuery object of the input that triggered the emitter, so you can compare the input with other fields.

## State Handlers

A state handler on a field acts when the form's state changes, and it usually shows or hides the field:

```php
'markers_draggable' => array(
    'type'       => 'checkbox',
    'default'    => false,
    'state_handler' => array(
        'map_type[interactive]' => array('show'),
        'map_type[static]' => array('hide'),
    ),
    'label' => __( 'Draggable markers', 'siteorigin-docs' )
),
```

This field is from the Google Maps Widget. The `markers_draggable` checkbox lets users drag the map's markers, which works only on an interactive map, so the handler hides the checkbox for a static map and shows it for an interactive map. Each entry in a state handler has this form:

```
'group[state]' => array( 'function', 'selector', array( 'args' ) ),
```

When `group` changes to `state`, the handler runs `function` on the jQuery object of the field's wrapper element. If `selector` isn't empty, the handler uses `jQuery.find()` to run the function on an element inside the wrapper, and `args` holds the function's arguments. This example colors the field's label green for an interactive map:

```php
'state_handler' => array(
    'map_type[interactive]' => array('css', 'label', array('color', '#00ff00') ),
    'map_type[static]' => array('css', 'label', array('color', '#474747') ),
),
```

### Running Several Actions

Add `[]` after the state name to run several actions for one state, and give the actions as an array of arrays:

```php
'state_handler' => array(
    'map_type[interactive][]' => array( 
        array( 'show' ),
        array( 'css', 'label', array( 'color', '#00ff00' ) ),
    ),
),
```

### Else Handlers

An `_else` handler runs for every state in a group that has no handler of its own. If an emitter sends several states and you want to hide a field for one of them, hide it for that state and show it in `_else`:

```php
'state_handler' => array(
    'map_type[interactive]' => array( 'hide' ),
    '_else[map_type]' => array( 'show' ),
),
```

The Widgets Bundle runs the handlers from top to bottom, and runs an `_else` handler only if no other handler for the same group has run.

## States in Repeaters

State groups apply to the whole form, so a state emitter on a field inside a repeater sends states to the same group from every item. Add `{$repeater}` to the group name to give each repeater item its own group.

The Contact Form Widget, for example, has a repeater of form fields. Each item has a **Field Type** select field and an **Options** repeater, which applies only to dropdown, checkbox and radio fields. The **Field Type** field's `select` emitter uses the group `field_type_{$repeater}`, and a state handler on the **Options** repeater shows or hides the repeater for the same group. Keep `{$repeater}` in a single-quoted PHP string, because in double quotes PHP reads `$repeater` as a variable. A state handler can list several states separated by commas, as in `[select,checkboxes,radio]` below.

```php
'type' => array(
	'type' => 'select',
	'label' => __( 'Field Type', 'siteorigin-docs' ),
	'options' => array(
		// Some options left out
		'text' => __( 'Text', 'siteorigin-docs' ),
		'select' => __( 'Dropdown Select', 'siteorigin-docs' ),
		'checkboxes' => __( 'Checkboxes', 'siteorigin-docs' ),
		'radio' => __( 'Radio', 'siteorigin-docs' ),
	),
	'state_emitter' => array(
		'callback' => 'select',
		'args' => array( 'field_type_{$repeater}' ),
	)
),
```

The **Options** repeater:

```php
'options' => array(
	'type' => 'repeater',
	'label' => __( 'Options', 'siteorigin-docs' ),
	'item_name' => __( 'Option', 'siteorigin-docs' ),
	'fields' => array(
		'value' => array(
			'type' => 'text',
			'label' => __( 'Value', 'siteorigin-docs' ),
		),
	),

	// These are only required for a few states
	'state_handler' => array(
		'field_type_{$repeater}[select,checkboxes,radio]' => array('show'),
		'_else[field_type_{$repeater}]' => array( 'hide' ),
	),
),
```
