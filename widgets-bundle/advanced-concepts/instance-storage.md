# Instance Storage

As the name implies, instance storage stores the current widget instance and makes it accessible through a hash string. This feature is useful when you want to access a variable without having it visible in your widgets HTML.

### When will you use it?

For example, lets say you create a newsletter widget and you're adding direct integration with a few newsletter providers. So you add a field that lets your users enter their MailChimp API key for example. You'll need to access this API key only after someone submits a form.

So, instead you'll just include the instance storage hash in your newsletter signup HTML form, and use that hash to retrieve the full instance, including the API key, after they've submitted that form back to your server.

### Enabling instance storage

Instance storage is not enabled by default, but you can enable it by setting the `instance_storage` key to true in the `$widget_options` array.

```php
class My_Newsletter_Widget extends SiteOrigin_Widget {
    function __construct() {

        parent::__construct(
            'sow-newsletter',
            __('Newsletter Widget', 'my-plugin'),
            array(
                // Enable instance storage
                'instance_storage' => true,
            ),
            array(

            ),
            array(
                'api_key' => array(
                    'type' => 'text',
                    'label' => __('API Key', 'my-plugin'),
                ),
            ),
            plugin_dir_path(__FILE__)
        );
    }
    
    // Rest of the widget goes here
}
```

The Widgets Bundle will enable instance storage for this specific widget. The Widgets Bundle will automatically cache the instance every time the widget is displayed.

### Using the $storage_hash variable

If your widget has `instance_storage` enabled, then your widget template files will have access to a variable called `$storage_hash`. You'll normally just add this as a hidden field in your form or as a `data-storage-hash` HTML attribute if you want to send requests using AJAX.

```
<input type="hidden" name="storage_hash" value="<?php echo esc_attr($storage_hash) ?>" />
```

### Retreiving Instance Storage

You're free to handle the user's form details however you want, but you'll need to call `$this->get_stored_instance( $storage_hash );`. The easiest way to handle a request would be to create an [ajax handler](https://codex.wordpress.org/AJAX_in_Plugins).

```php
function my_action_callback() {
    // You should do some nonce checking here
    if( empty($_POST['_wp_nonce']) || !wp_verify_nonce( $_GET['_wp_nonce'], 'action' ) ) return;
    
    $widget = new My_Newsletter_Widget;
    $instance = $widget->get_stored_instance( $_POST['storage_hash'] );
    $key = $instance['api_key'];
    
    // Now we can handle the rest of the input from $_POST.
    SomeNewsletterAPI::signup( $key, $_POST['email'] );
    
    // Either die or redirect the user to a success page
    wp_die();
}
add_action( 'wp_ajax_my_action', 'my_action_callback' );
```

You're not limited to using instance storage in this specific way though. Once you enable it, you'll be able to store the hash and retrieve the $instance wherever and whenever you please.