# Theme Integration

Page Builder works with most WordPress themes without any changes. This guide shows theme developers how to make Page Builder fit a theme's design more closely. The examples come from SiteOrigin themes such as [Corp](https://siteorigin.com/theme/corp/), [North](https://siteorigin.com/theme/north/) and [Vantage](https://siteorigin.com/theme/vantage/).

Put your Page Builder code in a separate file and load it only when Page Builder is active. Corp does this at the end of `functions.php`.

```php
if ( defined( 'SITEORIGIN_PANELS_VERSION' ) ) {
	require get_template_directory() . '/inc/siteorigin-panels.php';
}
```

## Declare Theme Support

Declare support for Page Builder with `add_theme_support()` in your `after_setup_theme` callback. The second argument is optional. It is an array of Page Builder settings, and the values you give become the defaults for this theme.

```php
function mytheme_setup() {
	add_theme_support( 'siteorigin-panels', array(
		'home-page'     => true,
		'margin-bottom' => 35,
		'title-html'    => '<h3 class="widget-title">{{title}}</h3>',
	) );
}
add_action( 'after_setup_theme', 'mytheme_setup' );
```

Page Builder clears its settings cache on `after_setup_theme` at priority 100. Declare support at a lower priority, such as the default of 10.

### How Theme Values Are Applied

Page Builder merges its settings in this order. Each step overrides the one before it.

1. Page Builder defaults. You can change these with the `siteorigin_panels_settings_defaults` filter.
2. Your theme support arguments.
3. The values saved on the **Settings > Page Builder** page.
4. The `siteorigin_panels_settings` filter.

When a user saves the **Settings > Page Builder** page, Page Builder saves every setting on that page to its settings option. After that, the saved values replace your theme values for those fields. Your theme values continue to apply to keys that aren't on the settings page, such as `home-page`, `home-page-default` and `home-template`.

To force a value that the user can't change, use the `siteorigin_panels_settings` filter. Use this with care. The settings page will still show the field, but your filter will override it.

```php
function mytheme_panels_settings( $settings ) {
	$settings['title-html'] = '<h2 class="widget-title">{{title}}</h2>';

	return $settings;
}
add_filter( 'siteorigin_panels_settings', 'mytheme_panels_settings' );
```

### Theme Support Keys

You can set any Page Builder setting through theme support. These are the keys that matter most to themes.

| Key | Default | Effect |
| --- | --- | --- |
| `home-page` | `false` | Adds the **Appearance > Home Page** screen. See **Custom Home Page** below. |
| `home-page-default` | `false` | The ID of the prebuilt layout that the Home Page screen loads when the site has no front page and no Page Builder home page yet. If this is empty, Page Builder uses the layout with the ID `home`, then the first layout it finds. |
| `home-template` | `'home-panels.php'` | The page template that Page Builder assigns to the home page. |
| `title-html` | `'<h3 class="widget-title">{{title}}</h3>'` | The HTML for widget titles. See **Widget Titles** below. |
| `add-widget-class` | `true` | Adds the `widget` class to each widget wrapper. |
| `responsive` | `true` | Collapses rows and columns on smaller screens. |
| `tablet-layout` | `false` | Adds a tablet collapse point for rows with three or more columns. |
| `tablet-width` | `1024` | The tablet collapse point, in pixels. |
| `mobile-width` | `780` | The mobile collapse point, in pixels. |
| `margin-bottom` | `30` | The space below rows and widgets, in pixels. |
| `margin-sides` | `30` | The gutter between columns, in pixels. |
| `row-mobile-margin-bottom` | `''` | The space below rows on mobile, in pixels. |
| `mobile-cell-margin` | `30` on new sites | The space between collapsed columns on mobile, in pixels. |
| `widget-mobile-margin-bottom` | `''` | The space below widgets on mobile, in pixels. |
| `margin-bottom-last-row` | `false` | Keeps the bottom margin on the last row. |
| `full-width-container` | `'body'` | The element that full width rows stretch to. See **Full Width Rows** below. |
| `post-types` | `array( 'page', 'post' )` | The post types that can use Page Builder. |

For the full list, see `settings_defaults()` in `inc/settings.php` in the Page Builder plugin.

North passes a theme setting into Page Builder. When the user turns off the theme's responsive layout, Page Builder's responsive layout turns off too. This works until a user saves the Page Builder settings page, as explained above.

```php
add_theme_support( 'siteorigin-panels', array(
	'home-page'  => true,
	'responsive' => ! siteorigin_setting( 'responsive_disabled' ),
) );
```

## Page Templates

Page Builder replaces the post content through the `the_content` filter. Your templates must call `the_content()` for Page Builder layouts to appear. You don't need a special template for Page Builder pages.

Page Builder gives you these functions for use in templates.

- `siteorigin_panels_is_panel()` returns `true` on a singular post that has a Page Builder layout, and on the Page Builder home page.
- `siteorigin_panels_is_home()` returns `true` on a static front page that has a Page Builder layout.
- `siteorigin_panels_render( $post_id )` returns the HTML for a layout. Use it to output a layout outside `the_content()`.

These functions, and the body classes below, work with layouts built in the classic editor. They don't detect a SiteOrigin Layout Block in the block editor. The block renders as part of the post content.

To stop Page Builder from replacing the content for one call to `the_content`, use the `siteorigin_panels_filter_content_enabled` filter. Unwind does this on video posts: it renders the layout without the first Video Player Widget, then runs that HTML through `the_content` with the filter off so Page Builder doesn't replace it with the full layout.

```php
$content = get_the_content();

add_filter( 'siteorigin_panels_filter_content_enabled', '__return_false' );
echo apply_filters( 'the_content', $content );
remove_filter( 'siteorigin_panels_filter_content_enabled', '__return_false' );
```

## Custom Home Page

When `home-page` is `true`, Page Builder adds **Appearance > Home Page**. There, users can build a home page and turn it on or off. If the front page doesn't have a Page Builder layout yet, Page Builder creates a page called "Home Page" when the user saves. When the user turns the home page on, Page Builder sets that page as the static front page.

If the page uses the default template, Page Builder assigns the template in the `home-template` key. It also applies that template to the home page when a user switches to your theme. Vantage ships a `home-panels.php` template for this. This is a simplified version of it.

```php
get_header();

echo '<div id="primary" class="content-area"><div class="entry-content">';

if ( is_page() ) {
	the_post();
	the_content();
} else {
	echo siteorigin_panels_render( 'home' );
}

echo '</div></div>';

get_footer();
```

When the Home Page screen creates a page, Page Builder fires the `siteorigin_panels_create_home_page` action with the new page ID. Use it to set up the page for your theme.

```php
function mytheme_create_home_page( $page_id ) {
	update_post_meta( $page_id, 'mytheme_hide_title', true );
}
add_action( 'siteorigin_panels_create_home_page', 'mytheme_create_home_page' );
```

## Widget Titles

The `title-html` setting controls the HTML around widget titles. Page Builder replaces `{{title}}` with the title. The HTML before `{{title}}` becomes the widget's `before_title` argument. The HTML after it becomes `after_title`. If the setting doesn't contain `{{title}}`, Page Builder uses `<h3 class="widget-title">` and `</h3>`.

Set `title-html` through theme support to match the titles in your widget areas. To change the arguments for each widget, use the `siteorigin_panels_widget_args` filter.

```php
function mytheme_panels_widget_args( $args ) {
	$args['before_title'] = '<h2 class="widget-title">';
	$args['after_title'] = '</h2>';

	return $args;
}
add_filter( 'siteorigin_panels_widget_args', 'mytheme_panels_widget_args' );
```

Page Builder adds the `widget` class to each widget wrapper by default. If your sidebar widget styles break widgets in Page Builder layouts, set `add-widget-class` to `false`. You can also use the `siteorigin_panels_default_add_widget_class` filter to change its default.

## Full Width Rows

The **Row Layout** setting in each row's **Layout** section has three options.

- **Standard** keeps the row inside the theme's content area.
- **Full Width** stretches the row background to the full width. The row content stays at the content width.
- **Full Width Stretched** stretches the row background and its content to the full width.

Page Builder can stretch rows in two ways. By default, it uses JavaScript. If your theme tells Page Builder about its content container, Page Builder uses CSS instead.

### JavaScript Stretching

Page Builder adds the `siteorigin-panels-stretch` class and a `data-stretch-type` attribute to a stretched row. A script then measures the row and sets negative side margins, so the row fills the full width container. For **Full Width** rows, it also adds side padding to keep the content at its original width.

The full width container is `body` by default. Users can change it with the **Full Width Container** field on the settings page. Themes can set the `full-width-container` key or use the `siteorigin_panels_full_width_container` filter. Vantage stretches rows to its `#main` element.

```php
function mytheme_panels_full_width_container() {
	return '#main';
}
add_filter( 'siteorigin_panels_full_width_container', 'mytheme_panels_full_width_container' );
```

Before the script runs, the `body` has the `siteorigin-panels-before-js` class. While this class is present, Page Builder's CSS gives stretched rows large negative margins and matching padding, and sets `overflow-x: clip` on the `body`. This reduces the layout shift when the script runs. This CSS is part of the default flexbox layout engine. Page Builder doesn't add it when the theme uses the CSS container breaker.

### CSS Container Breaker

If your theme has a content container with a fixed maximum width, give Page Builder its selector and width. Page Builder then stretches rows with CSS instead of the stretch script. Both filters must return a value.

```php
function mytheme_panels_container_selector() {
	return '.site-content .container';
}
add_filter( 'siteorigin_panels_theme_container_selector', 'mytheme_panels_container_selector' );

function mytheme_panels_container_width() {
	return '1200px';
}
add_filter( 'siteorigin_panels_theme_container_width', 'mytheme_panels_container_width' );
```

When both filters return a value, Page Builder does the following.

- Adds the `siteorigin-panels-css-container` class to the `body`.
- Wraps the cells of each **Full Width** row in a `.so-panels-full-wrapper` element.
- On pages with a full width row, removes the `max-width`, side padding and side margins from your container.
- On those pages, gives `.so-panels-full-wrapper`, `.panel-grid.panel-no-style` and `.panel-row-style:not([data-stretch-type])` your width as `max-width` and centers them.

The result is that standard rows stay at your content width, and stretched rows fill the container. This CSS is part of the default flexbox layout engine. If the **Use Legacy Layout Engine** setting selects the legacy engine, the filters still turn off JavaScript stretching, but Page Builder doesn't add this CSS. Other content in the container, such as the page title, also loses its `max-width` on those pages. Style that content so it stays at your content width.

### Full Width Page Layouts

Full width rows can only stretch as wide as the full width container. If your theme has a sidebar or a narrow content area, give users a way to remove it on Page Builder pages. See **Page Settings** below.

## Page Settings

SiteOrigin themes use a **Page Settings** meta box to let users remove the sidebar, the title and the spacing around a page. This meta box is part of the theme, not Page Builder. It works well with Page Builder because a layout often needs the full content area.

Corp adds these settings to pages and posts.

- **Page Layout**: Default, No Sidebar, or Full Width, No Sidebar.
- **Header Overlap**: places the header over the content.
- **Header** and **Footer**: show or hide them.
- **Header Bottom Margin** and **Footer Top Margin**: remove the space between the header, the content and the footer.
- **Page Title**: shows or hides the title.
- **Footer Widgets**: shows or hides the footer widgets.

North has similar settings. Its **Page Layout** options are Default, No Sidebar, Full Width, Full Width, With Sidebar, and Stripped. It also has **Page Title**, **Masthead Bottom Margin**, **Footer Top Margin**, **Hide Masthead** and **Hide Footer Widgets**.

The themes use these values in two ways. First, templates check a value before they output an element. This is from Corp's `template-parts/content-page.php`.

```php
if ( siteorigin_page_setting( 'page_title' ) ) {
	the_title( '<h1 class="entry-title">', '</h1>' );
}
```

Second, the theme adds body classes, and the stylesheet removes spacing for those classes. Corp adds `page-layout-no-sidebar`, `page-layout-full-width-no-sidebar`, `no-header-margin` and `no-footer-margin`. Its `sidebar.php` template doesn't output the sidebar when the layout isn't Default. Its CSS then widens the content area and removes the container width or the margins.

```css
.page-layout-full-width-no-sidebar .site-content .corp-container {
	max-width: none;
	padding: 0;
}

.no-header-margin .site-header {
	margin-bottom: 0;
}
```

The themes also set up the page that the Home Page screen creates. Their settings code hooks into the `siteorigin_panels_create_home_page` action and saves the default page settings for the new page. North, Unwind and Vantage change those defaults to no sidebar and no page title.

If you don't need a meta box, you can target Page Builder pages with the `siteorigin-panels` body class. See **Body Classes** below.

## Row, Column and Widget Styles

Themes can add their own fields to the row, column and widget style settings. Use these filters.

- `siteorigin_panels_row_style_fields`
- `siteorigin_panels_cell_style_fields`
- `siteorigin_panels_widget_style_fields`

Page Builder puts any field without a `group`, or with `'group' => 'theme'`, in a **Theme** section. Add your theme fields there so users can tell them apart from Page Builder's own fields.

```php
function mytheme_panels_row_style_fields( $fields ) {
	$fields['top_border'] = array(
		'name'     => __( 'Top Border Color', 'mytheme' ),
		'type'     => 'color',
		'group'    => 'theme',
		'priority' => 3,
	);

	return $fields;
}
add_filter( 'siteorigin_panels_row_style_fields', 'mytheme_panels_row_style_fields' );

function mytheme_panels_row_style_attributes( $attributes, $style ) {
	if ( ! empty( $style['top_border'] ) ) {
		$attributes['style'] .= 'border-top: 1px solid ' . esc_attr( $style['top_border'] ) . ';';
	}

	return $attributes;
}
add_filter( 'siteorigin_panels_row_style_attributes', 'mytheme_panels_row_style_attributes', 10, 2 );
```

This example is based on Vantage, which adds its legacy row options to the **Theme** section. Page Builder also calls the fields filters when it sanitizes styles. In that call, `$post_id` and `$args` are `false`, so always return your fields.

For the field types and the other style filters, see [Custom Row Options](./hooks/filtering-row-styles.md) and [Filtering Widget Options](./hooks/filtering-widget-styles.md).

## Prebuilt Layouts

Themes can bundle layouts that users insert from **Layouts > Prebuilt Layouts**. Page Builder looks for JSON layout files in a `siteorigin-page-builder-layouts` folder in the parent theme and in the child theme. To use another folder, use the `siteorigin_panels_local_layouts_directories` filter. Corp keeps its layouts in `inc/layouts`.

```php
function siteorigin_corp_layouts_folder( $layout_folders ) {
	$layout_folders[] = get_template_directory() . '/inc/layouts';

	return $layout_folders;
}
add_filter( 'siteorigin_panels_local_layouts_directories', 'siteorigin_corp_layouts_folder' );
```

You can also add layouts as PHP arrays with the `siteorigin_panels_prebuilt_layouts` filter. For the file format, thumbnails and sorting, see [Bundling Prebuilt Layouts](./bundling-prebuilt.md).

## Body Classes

Page Builder adds these classes to the `body`.

| Class | When |
| --- | --- |
| `siteorigin-panels` | The page has a Page Builder layout. |
| `siteorigin-panels-before-js` | The page has a Page Builder layout. A script removes it when the page loads. |
| `siteorigin-panels-home` | The page is the Page Builder home page. |
| `siteorigin-panels-live-editor` | The page is shown in the Live Editor. |
| `siteorigin-panels-css-container` | The theme uses the CSS container breaker. |

Use `siteorigin-panels` to change your theme's layout on Page Builder pages.

```css
.siteorigin-panels .entry-content {
	margin-top: 0;
}
```

Inside the layout, Page Builder uses classes such as `panel-layout`, `panel-grid`, `panel-grid-cell` and `so-panel`. For the full structure and the filters that change it, see [HTML Structure](./hooks/html.md). For layout CSS, see [CSS](./hooks/css.md).

## Widget Areas and the Customizer

Page Builder adds a **Layout Builder** widget. Users can add it to any widget area to build a layout there. It works in **Appearance > Widgets** and in the Customizer. Your theme doesn't need extra code for this. Make sure your widget areas can hold columns, or document which areas suit the widget.

Page Builder doesn't add Customizer settings of its own. To connect a theme option to Page Builder, pass it through theme support, as North does with its responsive setting.

Page Builder also has a **Sidebars Emulator** setting, which is on by default. It registers the widgets in each classic editor layout as if they were in a widget area. This helps widgets that check `is_active_widget()` before they load scripts or styles.

## Post Loop Widget Templates

Page Builder's Post Loop widget uses templates from your theme. It lists files that match `loop*.php` and `content*.php` in the theme root and one folder down, in both the parent and child theme.

Remove files that aren't complete loops with the `siteorigin_panels_postloop_templates` filter. Corp does this for its template parts.

```php
function mytheme_filter_post_loop_templates( $templates ) {
	$disallowed = array(
		'template-parts/content.php',
		'template-parts/content-page.php',
		'template-parts/content-single.php',
	);

	return array_diff( $templates, $disallowed );
}
add_filter( 'siteorigin_panels_postloop_templates', 'mytheme_filter_post_loop_templates' );
```

To add a template from a deeper folder, add its path relative to the theme folder with the same filter, for example `'partials/loops/loop-grid.php'`.

## Other Filters for Themes

These filters are also useful for themes.

- `siteorigin_panels_css_row_gutter` and `siteorigin_panels_css_row_margin_bottom` change the gutter and row spacing for each row. See [CSS](./hooks/css.md).
- `siteorigin_panels_layout_classes`, `siteorigin_panels_row_classes`, `siteorigin_panels_cell_classes` and `siteorigin_panels_widget_classes` add classes to the layout, rows, columns and widgets. See [HTML Structure](./hooks/html.md).
- `siteorigin_panels_widgets` and `siteorigin_panels_widget_dialog_tabs` group your theme's widgets and add a tab for them in the Add Widget dialog. See [Widget Groups](./widget-groups.md).
