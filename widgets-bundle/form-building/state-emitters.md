# Modifying Forms With State Emitters

State emitters give you a way to easily hide, show or modify certain parts of a widget's form. Using entries in the form array, you can define state emitters fields, that will emit a specific state to the rest of the form, and then state handlers that will perform a specific action based on any changes in the state.

State emitters are a fairly advanced concept, and won't be necessary for all fields. Make sure you're fairly comfortable with creating forms before you try adding state emitters.

## Form States

A form state is scoped to a given widget form. Each form can only occupy one state per state group. A state is represented by the string `group[state]`. Group and state names can consist of alphabetical characters and underscores.

## State Emitters

The first thing you need to understand is state emitters. These are tiedto specific fields. They broadcast form states based on a given set of arguments. Here's how you define them in the form array.

```php
'map_type'    => array(
    'type'    => 'radio',
    'default' => 'interactive',
    'label'   => __( 'Map type', 'siteorigin-widgets' ),
    'state_emitter' => array(
        'callback' => 'select',
        'args' => array( 'map_type' )
    ),
    'options' => array(
        'interactive' => __( 'Interactive', 'siteorigin-widgets' ),
        'static'      => __( 'Static image', 'siteorigin-widgets' ),
    )
),
```

What the `state_emitter` argument is essentially saying is that every time this field changes, we'll emit a new state based on what's returned by the state emitter callback.

The Widgets Bundle has a few built in state emitter callbacks that should cover most of your needs.

### The select Callback

The `select` state emitter simply sets the state for all the given groups in the args array to the value of the field. This is especially useful when you want to set the state of the field based on a dropdown or radio field.

### The in Callback

The `in` state emitter checks if the current field value is in the set of arguments.

```php
'state_emitter' => array(
    'callback' => 'in',
    'args' => array(
        'group[state_1]: option1, option2',
        'group[state_2]: option3, option4',
    )
),
```

This is useful when you want to group different options into a set of states.

### The conditional Callback

The conditional callback evaluates arbitrary conditions. The variable `var` is available to use in any way you please. Your expression should be valid Javascript and evaluate to a boolean.

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

If you need a custom state emitter for some specialized functionality, you can add it in Javascript by simply attaching a new function to the global `sowEmitters` object.

```javascript
sowEmitters.custom : function(val, args){
    var returnStates = {};
    
    // Use the field value in val and the args to set the group states
    returnStates['group'] = 'state';
    
    return returnStates;
},
```

## State Handlers

Once a state has been emitted by any field in the form, the rest of the form can handle state changes by defining a state handler. The most common use case of this will be to show or hide fields, depending on the state of the form.

```php
'markers_draggable' => array(
    'type'       => 'checkbox',
    'default'    => false,
    'state_handler' => array(
        'map_type[interactive]' => array('show'),
        'map_type[static]' => array('hide'),
    ),
    'label' => __( 'Draggable markers', 'siteorigin-widgets' )
),
```

This particular field is from the Google Maps widget. The `markers_draggable` lets you decide if you want the user to be allowed to move the markers. This feature, however, is only available in interactive map mode, not static map mode.

So we're using this state handler to hide this checkbox field if the maps is a static map and show it if it's an interactive map. Each value in the state handler array is essentially saying the following.

```
'group[state]' => array('function', 'selector', array( 'args ) ),
```

So when the form changes to `state` in `group`, we'll run the given action. The `function` is run on the jQuery object of the form wrapper. If `selector` isn't empty, then we'll use it to target a sub-element using `jQuery.find`. The `args` array is the argument for the action function.

So as an example, if you want to change the color of a field's label to green in a given state, this is the state handler argument you would use.

```php
'state_handler' => array(
    'map_type[interactive]' => array('css', 'label', array('color', '#00ff00') ),
    'map_type[static]' => array('css', 'label', array('color', '#474747') ),
),
```

### Multiple actions

State handlers also have a very simple syntax for running multiple actions for a given state.

```php
'state_handler' => array(
    'map_type[interactive][]' => array( 
        array( 'show' ),
        array( 'css', 'label', array('color', '#00ff00' ),
    ),
),
```

Just adding the `[]` after the state name is enough to tell the Widgets Bundle that there are multiple actions to run. It just expects an array of arrays.

### Else arguments

Else arguments give you a catch all of actions to run for a given group. Let's say, for example, you've created a state emitters that emits several states for a given goup. You only want to hide a field for one of those states and show it for the rest. Rather than defining actions for each state, you can combine them all into a single `_else`.

```php
'state_handler' => array(
    'map_type[interactive]' => array( 'hide' ),
    '_else[map_type]' => array( 'show' ),
),
```

The Widgets bundle goes from top to bottom of the state handlers. If it reaches an else handler, it'll run it only if no actions have run for the same group for the given state handler. 