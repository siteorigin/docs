# Version Update Action

The `siteorigin_widgets_version_update` action runs on the first admin request, at the `admin_init` action, after the Widgets Bundle is installed or updated. On a new install, `$old_version` is `false`. The Widgets Bundle uses the action for its own housekeeping, such as clearing its CSS cache, and your extension can use it to act when the Widgets Bundle updates.

```php
function myplugin_bundle_version_change($new_version, $old_version){
    // We can process the version change here.
}
add_action('siteorigin_widgets_version_update', 'myplugin_bundle_version_change', 10, 2);
```
