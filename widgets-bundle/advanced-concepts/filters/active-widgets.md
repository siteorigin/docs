# Active Widgets Filter

The `siteorigin_widgets_active_widgets` filter changes which widgets are active. To activate a widget from code, `SiteOrigin_Widgets_Bundle::single()->activate_widget( $id )` is the more direct route. The filter suits other cases, such as keeping all your own widgets active whatever users set at **Plugins > SiteOrigin Widgets**.

The keys of the `$active` array are widget folder names, such as `button`. A base ID, such as `sow-button`, doesn't work. Set each key to `true` or `false`.

```php
function wbexample_filter_active_widgets( $active ) {
	$active['wbe-staff'] = true;
	return $active;
}
add_filter( 'siteorigin_widgets_active_widgets', 'wbexample_filter_active_widgets' );
```
