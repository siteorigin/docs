# LESS stylesheets
For easier development of styles and runtime stylesheet generation the Widgets Bundle uses LESS. A LESS stylesheet may be specified by overriding the `get_style_name()` function and returning the name of the file, without the `.less` extension, found in the `styles` folder.

## Mixin libraries
For convenience, we have included the mixin libraries, <a href="http://lesselements.com/" target="_blank">LESS Elements</a> and <a href="http://lesshat.madebysource.com/" target="_blank">LESSHat</a>, in the `base/less/` folder. They help reduce the amount of CSS required to ensure compatibility with multiple versions of multiple browsers, or where CSS is simply too verbose. See their respective documentation pages for more information on what's available and usage examples. These may be included in a LESS stylesheet by using the `@import` directive, as follows:

```less
@import "mixins";
@import "lesshat";
```

## Importing additional files
Other LESS and CSS files may be included using the `@import` directive. These additional files should be placed in the widget's `styles` folder. They will then be included before LESS compilation takes place.

## Injecting LESS variables
For runtime stylesheet generation, the `SiteOrigin_Widget` base class provides a `get_less_variables()` function. To inject variables into your LESS stylesheets you provide the injection point in the stylesheet simply by declaring the variable name and a default value, and then supply those variables in an array returned by `get_less_variables()`. They keys in the returned array must match the LESS variable names exactly. 

### Example - injecting LESS variables
Provide injections points in the LESS stylesheet:
```less
@background_color: #ffffff;
@border_radius: 5px;

.some_class {
    background-color: @background_color;
    border-radius: @border_radius;
}
```

Supply the variables in your widget class:
```php
function get_less_variables( $instance ) {
    return array(
        'background_color' => $instance['background_color'],
        'border_radius' => $instance['border_radius'],
    );
}
```

## LESS `.widget-function()` callback
The Widgets Bundle allows callbacks from LESS files which may generate additional runtime styles based on user inputs. This is done in LESS by calling the `.widget-function()` function with the first argument being the name of the function to call, and any subsequent arguments are passed through to the function being called. In your widget class, you supply the function with a name prepended by 'less_'.

### Example - using the `.widget-function()` callback
In the LESS stylesheet, call `.widget-function()`:
```less
.widget-function( 'my_widget_function', 'use_blue' );
```

Then in your widget class you supply the function prepended by 'less_'
```php
function less_my_widget_function( $instance, $args ) {
    return $args[0] == 'use_blue' ? '#0000ff' : '#ff0000';
}
```

