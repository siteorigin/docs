# Page Builder Prebuilt Layouts

Page Builder makes it easy to bundle prebuilt layouts into your theme or plugin. We've made a point of not including any of these into the Page Builder core, relying mainly on other themes and plugins to add the layouts that work with their unique designs.

## Register a Custom Layouts Folder Location

Use the below function to register a location for your prebuilt layout files. Replace `siteorigin` with your theme or plugin namespace. Change folder path as required, in this example we're storing our layouts in the `/inc/layouts` folder

```
/**
 * Register a custom layouts folder location.
 */
function siteorigin_layouts_folder( $layout_folders ) {
	$layout_folders[] = get_template_directory() . '/inc/layouts';
	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'siteorigin_layouts_folder' );
```

### Export Your Layout JSON File

Edit the page containing your layout. In Page Builder, click Layouts > Import/Export > Download Layout. To change your layout name as seen in Page Builder, edit the JSON file and locate the name/value pair at the end of the file `"name":"Home"`. Change the `name` value as required. In this example, our layout is named `Home`. To add a thumbnail for each layout, include a JPG or PNG file with the same filename as the JSON file you'd like to use it for. For example, if your JSON file was named `home.json`, your thumbnail image would be named `home.jpg`. Finally, copy both the JSON layout file and thumbnail to your layouts directory location. Navigate to Layouts > Prebuilt layouts in any Page Builder page to view and test your layout.

### External Images

If you'd like your layout images to populate on other domains, make use of the External URL field wherever possible. For example, when adding a row background you can either use the Select Image button or insert a URL into the External URL field. Using the External URL field will ensure the image loads on domains other than your own.

---

### Example Theme

You can find an example theme [here](https://siteorigin.com/wp-content/uploads/2019/11/starter-theme.zip). `starter-theme` was created from underscores and isn't intended for production usage. Within the example theme you'll find the following:

#### functions.php

At the end of the theme's `functions.php` file we conditionally require a Page Builder compatibility file.

```
/**
 * Page Builder by SiteOrigin compatibility file.
 */
if ( defined( 'SITEORIGIN_PANELS_VERSION' ) ) {
	require get_template_directory() . '/inc/siteorigin-page-builder.php';
}
```

#### /inc/siteorigin-page-builder.php

In our compatibility file we register the location of our prebuilt layouts folder.

```
/**
 * Register a custom layouts folder location.
 */
function starter_theme_layouts_folder( $layout_folders ) {
	$layout_folders[] = get_template_directory() . '/inc/layouts';
	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'starter_theme_layouts_folder' );
```

#### /inc/layouts/

In our layouts folder we've included a demo layout and matching thumbnail.

---

### Example Child Theme

You can find an example child theme [here](https://siteorigin.com/wp-content/uploads/2019/11/siteorigin-corp-child-prebuilt-layouts.zip). Our child theme uses [SiteOrigin Corp](https://siteorigin.com/theme/corp/) as its parent theme. Within the example child theme you'll find the following:

#### functions.php

In the child theme `functions.php` file we register the location of our prebuilt layouts folder.

```
/**
 * Register a custom layouts folder location.
 */
function siteorigin_corp_child_layouts_folder( $layout_folders ) {
	$layout_folders[] = get_stylesheet_directory() . '/layouts';
	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'siteorigin_corp_child_layouts_folder' );
```
#### layouts

In our layouts folder we've included a demo layout and matching thumbnail.

---

### Example Plugin

You can find an example theme [here](https://siteorigin.com/wp-content/uploads/2019/11/so-prebuilt-layouts.zip). Within the example plugin you'll find the following:

#### so-custom-post-loop.php

In the main plugin file we register the location of our prebuilt layouts folder.

```
/**
 * Register a custom layouts folder location.
 */
function so_prebuilt_layouts_folder( $layout_folders ) {
	$layout_folders[] = plugin_dir_path( __FILE__ ) . '/layouts';
	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'so_prebuilt_layouts_folder' );
```

#### layouts

In our layouts folder we've included a demo layout and matching thumbnail.
