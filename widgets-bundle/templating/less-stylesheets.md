# LESS Stylesheets

The Widgets Bundle compiles each widget's LESS stylesheet when the widget renders, so the CSS can use the widget's settings. It loads `styles/default.less` from your widget folder. Override `get_style_name()` to use another stylesheet in the `styles` folder, and return the file's name without `.less`.

The Widgets Bundle saves the compiled CSS in `wp-content/uploads/siteorigin-widgets/`. While you test your LESS, add this line to your `wp-config.php` file to stop the Widgets Bundle saving the CSS:

`define( 'SITEORIGIN_WIDGETS_DEBUG', true );`

## Mixin Libraries

The Widgets Bundle includes two mixin libraries in its `base/less/` folder, LESS Elements and [LESSHat](https://github.com/madebysource/lesshat). Their mixins write vendor-prefixed and repetitive CSS for you, and each library's documentation lists its mixins. Import them with `@import`:

```less
@import "mixins";
@import "lesshat";
```

## Importing Other Files

`@import` also includes other LESS and CSS files, which the Widgets Bundle adds before it compiles the stylesheet. Put the files in the widget's `styles` folder, and give each `@import` its own line with the file name in double quotes.

## LESS Variables

`get_less_variables()` passes values from the widget into your stylesheet. Declare each variable in the stylesheet with a default value, then return the values from `get_less_variables()` in an array whose keys match the LESS variable names exactly.

### Example: Passing Variables

Declare the variables in the LESS stylesheet:

```less
@background_color: #ffffff;
@border_radius: 5px;

.some_class {
    background-color: @background_color;
    border-radius: @border_radius;
}
```

Return the values from your widget class:

```php
function get_less_variables( $instance ) {
    return array(
        'background_color' => $instance['background_color'],
        'border_radius' => $instance['border_radius'],
    );
}
```

The Widgets Bundle skips empty strings, `false` and `null`, so the stylesheet's default applies. It also skips a value that references another LESS variable, uses `@{}` interpolation or `data-uri()`, adds `!important` or holds more than one declaration. Define `SITEORIGIN_WIDGETS_DEBUG` as `true` to get a PHP notice for each skipped value.

## The `.widget-function()` Callback

`.widget-function()` calls a PHP method of your widget from the stylesheet, so the method can output styles based on the widget's settings. The first argument is the method's name, and the Widgets Bundle passes the other arguments to the method. Name the method in your widget class with a `less_` prefix.

### Example: Using `.widget-function()`

Call `.widget-function()` in the LESS stylesheet:

```less
.some_class {
    .widget-function('my_widget_function', use_blue);
}
```

Don't put a space before the method name, and don't quote the other arguments. The Widgets Bundle passes them as strings, with only the surrounding whitespace removed.

Then add the method, with the `less_` prefix, to your widget class. The returned string replaces the whole `.widget-function()` statement, so return complete LESS declarations:

```php
function less_my_widget_function( $instance, $args ) {
    $color = ( isset( $args[0] ) && $args[0] === 'use_blue' ) ? '#0000ff' : '#ff0000';
    return 'color: ' . $color . ';';
}
```
