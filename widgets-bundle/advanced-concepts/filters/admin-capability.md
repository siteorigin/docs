# Menu Capability Filter

This filter just gives you the chance to change the [capability](https://developer.wordpress.org/plugins/users/roles-and-capabilities/) required to view Plugins > SiteOrigin Widgets. Users with this capability can also activate and deactivate widgets, and open and save the widget settings forms. By default, the capability is `manage_options`.

```php
function wbe_widgets_capability_filter($cap){
	$cap = 'edit_pages';
	return $cap;
}
add_filter('siteorigin_widgets_admin_menu_capability', 'wbe_widgets_capability_filter');
```