# Filtering Page Builder Features and Actions

The `siteorigin_panels_builder_supports` filter enables and disables Page Builder's features and the actions users can take on rows and widgets. Every feature and action is enabled by default. The filter also passes `$post` and `$panels_data`.

This example stops users from adding new widgets:

```php
function so_disallow_new_widgets( $supports ) {
	$supports['addWidget'] = false;

	return $supports;
}
add_filter( 'siteorigin_panels_builder_supports', 'so_disallow_new_widgets' );
```

## Actions

- `addRow`: users can add new rows and duplicate rows.
- `editRow`: users can edit rows.
- `deleteRow`: users can delete rows.
- `moveRow`: users can move rows.
- `addWidget`: users can add new widgets.
- `editWidget`: users can edit widgets.
- `deleteWidget`: users can delete widgets.
- `moveWidget`: users can move widgets.

## Features

- `prebuilt`: the **Layouts** button in the Page Builder toolbar opens the prebuilt layouts.
- `history`: the **History** button in the toolbar lets users undo earlier edits.
- `liveEditor`: the **Live Editor** button in the toolbar opens the Live Editor.
- `revertToEditor`: users can switch from Page Builder back to the WordPress editor.

Page Builder applies `history`, `liveEditor` and `revertToEditor` only in a builder that shows the matching button.

## Layout Builder Widget

The `siteorigin_panels_layout_builder_supports` filter does the same job for the Layout Builder widget and leaves the main Page Builder untouched. Its second argument is the Layout Builder's `$panels_data` as a JSON string. This example stops users from adding new widgets inside Layout Builder widgets:

```php
function so_disallow_new_widgets_layout_builder( $supports ) {
	$supports['addWidget'] = false;

	return $supports;
}
add_filter( 'siteorigin_panels_layout_builder_supports', 'so_disallow_new_widgets_layout_builder' );
```
