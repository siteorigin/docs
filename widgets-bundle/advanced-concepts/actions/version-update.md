# Version update action

Widgets Bundle triggers this action whenever users update the core Widgets Bundle plugin. It's used internally to perform housekeeping functions like cleaning old CSS cache. There aren't any clear use cases outside the core, but if your extension needs to know when the core has been updated, the action is available.

```php
function myplugin_bundle_version_change($new_version, $old_version){
    // We can process the version change here.
}
add_action('siteorigin_widgets_version_update', 'myplugin_bundle_version_change', 10, 2);
```