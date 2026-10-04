# Menu Capability Filter

The `siteorigin_widgets_admin_menu_capability` filter changes the [capability](https://developer.wordpress.org/plugins/users/roles-and-capabilities/) that users need to open **Plugins > SiteOrigin Widgets**. Users with the capability can also activate and deactivate widgets, and open and save widget settings. The capability is `manage_options` unless you change it.

```php
function wbe_widgets_capability_filter($cap){
	$cap = 'edit_pages';
	return $cap;
}
add_filter('siteorigin_widgets_admin_menu_capability', 'wbe_widgets_capability_filter');
```
