# Icons and Fonts

The Widgets Bundle adds its icon and font families to the icon and font fields through filters. Your theme or plugin can use the same filters to add its own families or replace ours.

## Icons

The Widgets Bundle includes these icon families, which you can use anywhere in your templates:

- [Font Awesome](https://fontawesome.com/)
- [IcoMoon](https://icomoon.io/)
- [Genericons](http://genericons.com/)
- [Typicons](http://typicons.com/)
- [Elegant Themes' Line Icons](http://www.elegantthemes.com/blog/freebie-of-the-week/free-line-style-icons)
- [Google Material Icons and Symbols](https://fonts.google.com/icons)
- [Ionicons](https://ionic.io/ionicons)

### Adding an Icon Family

The `siteorigin_widgets_icon_families` filter adds icon families. Each family is an item in an associative array, and the item's key is the family's name, which must not contain a hyphen. The item holds these keys:

- `name` (`string`): the family's name in the icon picker.
- `style_uri` (`string`): the URL of the family's stylesheet, which the Widgets Bundle enqueues wherever the family's icons appear.
- `icons` (`array`): an associative array with each icon's name as the key and its unicode value as the value.

The Widgets Bundle outputs each icon as `<span class="sow-icon-{family}" data-sow-icon="{unicode value}">`. Your stylesheet needs the `@font-face` declaration, a `font-family` rule for `.sow-icon-{family}` and `content: attr(data-sow-icon);` for `.sow-icon-{family}[data-sow-icon]:before`. The Widgets Bundle's `icons/genericons/style.css` is a working example.

```php
function my_icon_families_filter( $icon_families ) {
    $icon_families['radicons'] = array(
		'name' => __( 'My Rad Icons', 'example-text-domain' ),
		'style_uri' => plugin_dir_url( __FILE__ ) . 'icons/style.css',
		'icons' => array(
		    'my-rad-search-icon' => '&#xf101;',
		    'my-rad-close-icon' => '&#xf101;'
		    // Etc.
		),
    );
    return $icon_families;
}
add_filter( 'siteorigin_widgets_icon_families', 'my_icon_families_filter' );
```

### Showing Part of an Icon Family

Each of our icon families has its own filter, so you can show part of a family:

- `siteorigin_widgets_icons_fontawesome`
- `siteorigin_widgets_icons_icomoon`
- `siteorigin_widgets_icons_genericons`
- `siteorigin_widgets_icons_typicons`
- `siteorigin_widgets_icons_elegantline`
- `siteorigin_widgets_icons_materialicons`
- `siteorigin_widgets_icons_ionicons`

This example removes the Font Awesome `address-book` icon from the `$icons` array:

```php
function my_fontawesome_icons_filter( $icons ) {
    unset( $icons['address-book'] );
    return $icons;
}
add_filter( 'siteorigin_widgets_icons_fontawesome', 'my_fontawesome_icons_filter' );
```

### Outputting an Icon

`siteorigin_widget_get_icon()` turns the value of an icon field into the icon's HTML. Pass it the value from the widget instance and, optionally, an indexed array of inline styles and a title for the icon's `title` attribute. For a form with these fields:

```php
$form_options = array(
    'my_icon' => array(
        'type' => 'icon',
        'label' => __( 'My Icon', 'example-text-domain' )
    ),
    'my_icon_size' => array(
        'type' => 'number',
        'label' => __( 'My Icon Size', 'example-text-domain' )
    ),
    'my_icon_color' => array(
        'type' => 'color',
        'label' => __( 'My Icon Color', 'example-text-domain' )
    )
);
```

The template outputs the icon with its size and color:

```php
<?php
    $icon_styles = array();
    if ( ! empty( $instance['my_icon_size'] ) ) $icon_styles[] = 'font-size: ' . intval( $instance['my_icon_size'] ) . 'px';
    if ( ! empty( $instance['my_icon_color'] ) ) $icon_styles[] = 'color: ' . $instance['my_icon_color'];
?>
<div>
   <?php echo siteorigin_widget_get_icon( $instance['my_icon'], $icon_styles ); ?>
</div>
```

## Fonts

The font field lets users style text with a web-safe font (Arial, Courier New, Georgia, Helvetica Neue, Lucida Grande or Times New Roman) or one of the Google Fonts.

### Adding a Font Family

The `siteorigin_widgets_font_families` filter adds font families. Add each font with its name as both the key and the value:

```php
function my_font_families_filter( $font_families ) {
    $font_families['A Really Cool Font'] = 'A Really Cool Font';
    return $font_families;
}
add_filter( 'siteorigin_widgets_font_families', 'my_font_families_filter' );
```

The Widgets Bundle loads only web-safe fonts and Google Fonts, so enqueue the stylesheet of any other font yourself. `siteorigin_widget_get_font()` passes the font array of these fonts through the `siteorigin_widget_get_custom_font_family` filter.

### Using a Font in a Template

A font reaches your widget's CSS through LESS variables. First, add a LESS stylesheet with variables for the font family, weight and style, as [LESS Stylesheets](../templating/less-stylesheets.md) describes:

```less
// Variable with default value where selected font family will be injected.
@font_family: "Lucida Grande", sans-serif;
@font_weight: 400;
@font_style: default;

.my-styled-text {
	// Use the font variables in a class.
	font-family: @font_family;
	font-weight: @font_weight;
	font-style: @font_style;
}
```

Then override `get_less_variables()` in your widget, and return the selected font's values from `siteorigin_widget_get_font()`:

```php
function get_less_variables( $instance ) {
    $selected_font = siteorigin_widget_get_font( $instance['some_font'] );
    $less_variables = array(
        'font_family' => $selected_font['family'],
    );
    if ( ! empty( $selected_font['weight'] ) ) {
        $less_variables['font_weight'] = $selected_font['weight_raw'];
        $less_variables['font_style'] = $selected_font['style'];
    }
    return $less_variables;
}
```

`siteorigin_widget_get_font()` returns an array with these items:

- `family`: the selected font family, such as `Alegreya`.
- `weight`: the selected weight and style together, such as `600italic`. The item is set only if the user selects a weight or an italic font. We keep it for backward compatibility, so use `weight_raw` and `style` in new code.
- `weight_raw`: the numeric weight, such as `600`. The item is set only if the user selects a weight.
- `style`: the selected style, such as `italic`. The item is empty unless the user selects an italic font.
- `url`: the Google Fonts stylesheet URL for the font, such as `https://fonts.googleapis.com/css?family=Alegreya:600italic`. The item is set only for Google Fonts.
