# Icons and fonts
For the icon and font fields types we use filters to add in the default icon and font families. This makes it possible for developers to hook into the same filters to replace or add in their own icon and font families.

## Icons
Icons are useful when trying to convey meaning in a small amount of space. The Widgets Bundle includes a few default icon families which may be used anywhere in your HTML templates. The default icon families are:
- <a href="http://fortawesome.github.io/Font-Awesome/" target="_blank">Font Awesome</a>
- <a href="https://icomoon.io/" target="_blank">IcoMoon</a>
- <a href="http://genericons.com/" target="_blank">Genericons</a>
- <a href="http://typicons.com/" target="_blank">Typicons</a>
- <a href="http://www.elegantthemes.com/blog/freebie-of-the-week/free-line-style-icons" target="_blank">Elegant Themes' Line Icons</a>.

### Filtering icons
To filter the icon families, you use the `'siteorigin_widgets_icon_families'` filter. An icon family is then added by including an associative array with the following properties:
- name: `string` The name of the icon family to be displayed when selecting an icon family.
- style_uri `string` The path of a CSS stylesheet to be enqueued when the icon family is to be displayed. This typically contains the `@font-face` declaration.
- icons: `array` An associative array containing `string` values, where the key is the name of the icon to be used and the value is it's unicode value.

```php
function my_icon_families_filter( $icon_families ) {
    $icon_families['radicons'] = array(
		'name' => __( 'My Rad Icons', 'example-text-domain' ),
		'style_uri' => plugin_dir_url( __FILE__ ) . '/icons/style.css',
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

We have also included filters for each of the default included icon families, so you may choose to only display a subset of the available icons in an icon family. The filters are:
- `'siteorigin_widgets_icons_fontawesome'` 
- `'siteorigin_widgets_icons_icomoon'` 
- `'siteorigin_widgets_icons_genericons'` 
- `'siteorigin_widgets_icons_typicons'` 
- `'siteorigin_widgets_icons_elegantline'` 

Let's say you don't want to display the FontAwesome 'glass' icon, then add the filter and remove it from the `$icons` array:
```php
function my_fontawesome_icons_filter( $icons ){
    if( isset( $icons['glass'] ) unset( $icons['glass'] );
    return $icons;
}
add_filter( 'siteorigin_widgets_icons_fontawesome', 'my_fontawesome_icons_filter' );
```

### Using icons
Once an icon has been selected for use in your widget form, it needs a bit of processing before it can be used in a template. The Widgets Bundle includes a function which does this for you, namely `siteorigin_widget_get_icon()`. To use it you simply pass in the selected value from the widget instance and, optionally, an indexed array of icon styles to be included inline.

Let's say your widget form includes the following:
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

Then, to render the icon in your template, you simply include a call to the icon rendering function:
```php
<?php
    $icon_styles = array();
    if(!empty($instance['my_icon_size'])) $icon_styles[] = 'font-size: '.intval($instance['my_icon_size']).'px';
    if(!empty($instance['my_icon_color'])) $icon_styles[] = 'color: '.$instance['my_icon_color'];
?>
<div>
   <?php echo siteorigin_widget_get_icon( $instance['my_icon'], $icon_styles ); ?>
</div>
```
 
## Fonts
Fonts provide a nice simple way to distinguish your site. The Widgets Bundle provides a way to style any text using the web safe fonts (Helvetica Neue, Lucida Grande, Georgia, and Courier New ) and a large selection of font families from the Google Fonts library.

>For the moment, using the font field is a fairly involved process which requires a few steps. We are working on simplifying this.
 
### Filtering fonts
To filter the default font families, you use the `'siteorigin_widgets_font_families'` filter. A font family is then added to the associative array by including the desired font name as both the key and the value. 

```php
function my_font_families_filter( $font_families ) {
    $font_families['A Really Cool Font'] = 'A Really Cool Font';
    return $font_families;
}
add_filter( 'siteorigin_widgets_font_families', 'my_font_families_filter' );
```
 
### Using fonts
To use the selected font family in your template, requires a few steps:

1) Include a LESS stylesheet as described [here](../templating/less-stylesheets.md). It will need at least a variable for the font family, but possibly one for font weight too.
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

2. Return the selected font family and weight in the widget's overridden `get_less_variables()` function, by using the `siteorigin_widget_get_font()`.

```php
function get_less_variables( $instance ) {
    $selected_font = siteorigin_widget_get_font( $instance['some_font'] );
    $less_variables = array(
        'font_family' => $selected_font['family'],
    );
    if( ! empty( $selected_font['weight'] ) ) {
        $less_variables['font_weight'] = $selected_font['weight_raw'];
        $less_variables['font_style'] = $selected_font['style'];
    }
    return $less_variables;
}
```

`siteorigin_widget_get_font` returns an array with following items:

**family**

The selected font family.

Example value:

`Alegreya`

**weight**

The selected font weight and styling. `weight` will only be set up the user selects a font with a weight, or the font is set to italic.

This item is maintained for backwards compatibility. We recommend using `widgets_raw` and `style` instead.

Example value:

`600italic`

**widgets_raw**

The selected font. `weight` will only be set up the user selects a font with a weight.

Example value:
`600`

**style**

The selected style. `style` will be empty if the user doesn't select an italic font option.

Example value:

`italic`
