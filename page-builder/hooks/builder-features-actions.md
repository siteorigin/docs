The `siteorigin_panels_builder_supports` filter allows you to enable and disable Page Builder features. For example, the following snippet will prevent users from being able to add new widgets:

```php
function so_disallow_new_widgets( $supports ) {
	$supports['addWidget'] = false;

	return $supports;
}
add_filter( 'siteorigin_panels_builder_supports', 'so_disallow_new_widgets' );
```

#### Adjustable Actions

All adjustable actions are enabled by default.

**editRow**

Controls whether rows are editable.

**deleteRow**

Controls whether rows are able to be deleted.

**moveRow**

Controls whether rows are able movable.

**addWidget**

Controls whether new widgets can be added.

**editWidget**

Controls whether widgets are able to be edited.

**deleteWidget**

Controls whether widgets are able to be deleted.

**moveWidget**

Controls whether widgets are able to be moved.

#### Adjustable Features

All adjustable features are enabled by default.

**prebuilt**

If `prebuilt` is enabled, the user will be able to access the Layouts Directory. The Layout Directory can accessed by clicking the Layouts button in the Page Builder toolbar.

**history**

If `prebuilt` is enabled, the History buttton will appear in the Page Builder toolbar and the user will be able to undo previous edits.

This action isn't supported by every Page Builder instance may not always have a noticeable effect.

**liveEditor**

If `prebuilt` is enabled, the Live Editor button will appear in the Page Builder toolbar, and the user will be able to make changes using the Live Editor.

This action isn't supported by every Page Builder instance may not always have a noticeable effect.

**revertToEditor**

This action isn't supported by every Page Builder instance may not always have a noticeable effect.

#### siteorigin_panels_layout_builder_supports

`siteorigin_panels_layout_builder_supports` is specific to the Layout Builder widget and is equivelent to `siteorigin_panels_builder_supports`. The following snippet will prevent new widgets from being added to layout builders - the base Page Builder instance will be unaffected.

```php
function so_disallow_new_widgets_layout_builder( $supports ) {
	$supports['addWidget'] = false;

	return $supports;
}
add_filter( 'siteorigin_panels_layout_builder_supports', 'so_disallow_new_widgets_layout_builder' );
```
