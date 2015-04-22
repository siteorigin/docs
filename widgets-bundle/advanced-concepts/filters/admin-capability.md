# Menu capability filter

This filter just gives you the chance to change the [capability](https://codex.wordpress.org/Roles_and_Capabilities) required to view Plugins > SiteOrigin Widgets. Users will also be able to activate and deactivate widgets. By default, the capability is `activate_plugins`.

```php
function wbe_widgets_capability_filter($cap){
	$cap = 'edit_pages';
	return $cap;
}
add_filter('siteorigin_widgets_admin_menu_capability', 'wbe_widgets_capability_filter')
```