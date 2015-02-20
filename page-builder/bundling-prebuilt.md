# Page Builder Prebuilt Layouts

Page Builder makes it easy to bundle prebuilt layouts into your theme or plugin. We've made a point of not including any of these into the Page Builder core, relying mainly on other themes and plugins to add the layouts that work with their unique designs.

### Enable Debug Mode

While in debug mode, Page Builder will show you some extra details about the page you're editing. To enable this, add the following lines to your `wp-config.php`.

```php
define('SITEORIGIN_PANELS_NOCACHE', true);
define('SITEORIGIN_PANELS_DEV', true);
```

### Page Builder Data Dump

After you've enabled debug mode, you'll get a full dump of your page's content as a PHP array. To do this, view the HTML source of a page builder page you've already created. Then search for the following string `Page Builder Data`. You'll see a standard HTML comment with the PHP for your page. Copy this array, we'll be using it shortly.

### Registering Your Prebuilt Layout

For this Page Builder uses the `siteorigin_panels_prebuilt_layouts` filter. So we'll create a function that hooks into this filter. The function takes an array of the existing layouts, adds its own layout (using a unique array key), then returns the entire array.

```php
function mytheme_prebuilt_layouts($layouts){
	$layouts['home-page'] = array(
		// We'll add a title field
		'name' => __('Default Home', 'vantage'),
		'widgets' => array( ... ),
		'grids' => array( ... ),
		'grid_cells' => array( ... )
	);
	return $layouts;
	
}
add_filter('siteorigin_panels_prebuilt_layouts','mytheme_prebuilt_layouts');
```

The layout array is just what we copied from the page builder data dump. We've added an extra `name` key, which Page Builder uses when it displays your prebuilt layout. 
