# Page Builder Prebuilt Layouts

Your theme or plugin can add prebuilt layouts to Page Builder, so users start a page from a design that fits your theme. Page Builder itself ships with no prebuilt layouts and leaves layouts to themes and plugins.

## Registering a Layouts Folder

Page Builder loads layouts from a `siteorigin-page-builder-layouts` folder in the parent theme and the child theme, with no code. To keep your layouts in another folder, register the folder with the `siteorigin_panels_local_layouts_directories` filter. Replace the `siteorigin` prefix with your theme or plugin's prefix, and change the path to your folder. This example registers the `inc/layouts` folder of a theme:

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

## Adding a Layout to the Folder

1. Build the layout on a page with Page Builder.
2. In Page Builder, click **Layouts**, open **Import/Export** and click **Download Layout** to save the layout as a JSON file.
3. Open the JSON file and find the `"name"` value at the end of the file, such as `"name":"Home"`. Change it to the name you'd like users to see in Page Builder.
4. To add a thumbnail, save a JPG, JPEG, GIF or PNG image with the same file name as the JSON file. For `home.json`, name the image `home.jpg`.
5. Copy the JSON file and the thumbnail to your layouts folder.
6. Open any page in Page Builder, click **Layouts** and open **Prebuilt Layouts** to check the layout.

## Using External Images

Wherever a layout uses an image, such as a row background, enter the image's address in the **External URL** field. Page Builder then loads the image from that address on every site that uses the layout, where an image chosen with **Select Image** would be missing from your users' Media Libraries.

## Sorting Layouts

Page Builder lists layouts in the order it finds them. The `siteorigin_panels_prebuilt_layouts` filter passes the `$layouts` array after Page Builder finds them, so you can sort it. This example sorts layouts alphabetically by their ID:

```
add_filter( 'siteorigin_panels_prebuilt_layouts', function( $layouts ) {
	// Sort layouts alphabetically.
	// uksort keeps the layout IDs, which Page Builder uses to find each layout.
	uksort( $layouts, 'strcmp' );

	return $layouts;
} );
```

## Examples

### Example Theme

The [example theme](https://siteorigin.com/wp-content/uploads/2019/11/starter-theme.zip), `starter-theme`, is based on Underscores and isn't meant for production sites. At the end of its `functions.php` file, the theme loads a Page Builder file when Page Builder is active:

```
/**
 * Page Builder by SiteOrigin compatibility file.
 */
if ( defined( 'SITEORIGIN_PANELS_VERSION' ) ) {
	require get_template_directory() . '/inc/siteorigin-page-builder.php';
}
```

That file, `inc/siteorigin-page-builder.php`, registers the theme's layouts folder:

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

The `inc/layouts` folder holds a demo layout and its thumbnail.

### Example Child Theme

The [example child theme](https://siteorigin.com/wp-content/uploads/2019/11/siteorigin-corp-child-prebuilt-layouts.zip) uses [SiteOrigin Corp](https://siteorigin.com/theme/corp/) as its parent theme. Its `functions.php` file registers the child theme's `layouts` folder, which holds a demo layout and its thumbnail:

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

### Example Plugin

The [example plugin](https://siteorigin.com/wp-content/uploads/2019/11/so-prebuilt-layouts.zip) registers its `layouts` folder in its main plugin file, `so-custom-post-loop.php`. The folder holds a demo layout and its thumbnail:

```
/**
 * Register a custom layouts folder location.
 */
function so_prebuilt_layouts_folder( $layout_folders ) {
	$layout_folders[] = plugin_dir_path( __FILE__ ) . 'layouts';
	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'so_prebuilt_layouts_folder' );
```
