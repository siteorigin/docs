# LESS Stylesheets

For easier development of styles and runtime stylesheet generation the Widgets Bundle uses LESS. By default, the Widgets Bundle loads `styles/default.less` from your widget folder. A different LESS stylesheet may be specified by overriding the `get_style_name()` function and returning the name of the file, without the `.less` extension, found in the `styles` folder.

Once the widget's LESS stylesheets are generated, they'll be cached at `wp-content/uploads/siteorigin-widgets/`. You can prevent this to make testing LESS simpler by adding the following to your `wp-config.php` file:

`define( 'SITEORIGIN_WIDGETS_DEBUG', true );`

## Mixin Libraries
For convenience, we have included the mixin libraries, LESS Elements and <a href="https://github.com/madebysource/lesshat" target="_blank">LESSHat</a>, in the `base/less/` folder. They help reduce the amount of CSS required to ensure compatibility with multiple versions of multiple browsers, or where CSS is simply too verbose. See their respective documentation pages for more information on what's available and usage examples. These may be included in a LESS stylesheet by using the `@import` directive, as follows:

```less
@import "mixins";
@import "lesshat";
```

## Importing Additional Files
Other LESS and CSS files may be included using the `@import` directive. These additional files should be placed in the widget's `styles` folder, and each `@import` directive must start its own line and use double quotes. They will then be included before LESS compilation takes place.

## Injecting LESS Variables
For runtime stylesheet generation, the `SiteOrigin_Widget` base class provides a `get_less_variables()` function. To inject variables into your LESS stylesheets you provide the injection point in the stylesheet simply by declaring the variable name and a default value, and then supply those variables in an array returned by `get_less_variables()`. They keys in the returned array must match the LESS variable names exactly. 

### Example - Injecting LESS Variables
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

Empty strings, `false` and `null` values are skipped, so the default value in the stylesheet applies. The Widgets Bundle also skips a value that references another LESS variable, uses `@{}` interpolation or `data-uri()`, adds `!important` or holds more than one declaration. Define `SITEORIGIN_WIDGETS_DEBUG` as `true` to get a PHP notice for each skipped value.

## LESS `.widget-function()` Callback
The Widgets Bundle allows callbacks from LESS files which may generate additional runtime styles based on user inputs. This is done in LESS by calling the `.widget-function()` function with the first argument being the name of the function to call, and any subsequent arguments are passed through to the function being called. In your widget class, you supply the function with a name prepended by 'less_'.

### Example - Using the `.widget-function()` Callback
In the LESS stylesheet, call `.widget-function()`:
```less
.some_class {
    .widget-function('my_widget_function', use_blue);
}
```

Don't put a space before the function name, and don't quote the other arguments. The Widgets Bundle passes them as strings with only the surrounding whitespace removed.

Then in your widget class you supply the function prepended by 'less_'. The returned string replaces the whole `.widget-function()` statement, so return complete LESS declarations:
```php
function less_my_widget_function( $instance, $args ) {
    $color = ( isset( $args[0] ) && $args[0] === 'use_blue' ) ? '#0000ff' : '#ff0000';
    return 'color: ' . $color . ';';
}
```

