# Instance Storage

As the name implies, instance storage stores the current widget instance and makes it accessible through a hash string. This feature is useful when you want to access a variable without having it visible in your widgets HTML.

### When Will You Use It?

For example, let's say you create a newsletter widget, and you're adding direct integration with a few newsletter providers. So you add a field that lets your users enter their MailChimp API key for example. You'll need to access this API key only after someone submits a form.

So, instead you'll just include the instance storage hash in your newsletter signup HTML form. You'd then use that hash to retrieve the full instance (including the API key) after a user has submitted the signup form back to your server.

### Enabling Instance Storage

Your widget won't have instance storage enabled by default. You can enable it by setting the `instance_storage` key to true in the `$widget_options` array.

```php
class My_Newsletter_Widget extends SiteOrigin_Widget {
	function __construct() {

		parent::__construct(
			'sow-newsletter',
			__( 'Newsletter Widget', 'siteorigin-docs' ),
			array(
				// Enable instance storage
				'instance_storage' => true,
			),
			array(

			),
			array(
				'api_key' => array(
					'type' => 'text',
					'label' => __( 'API Key', 'siteorigin-docs' ),
				),
			),
			plugin_dir_path( __FILE__ )
		);
	}

	// Rest of the widget goes here
}
```

The Widgets Bundle will enable instance storage for this specific widget. The Widgets Bundle will automatically cache the instance every time the widget is displayed.

### Using the $storage_hash Variable

If your widget has `instance_storage` enabled, then your widget template files will have access to a variable called `$storage_hash`. You'll normally just add this as a hidden field in your form or as a `data-storage-hash` HTML attribute if you want to send requests using AJAX.

```php
<input type="hidden" name="storage_hash" value="<?php echo esc_attr( $storage_hash ); ?>" />
<?php wp_nonce_field( 'my_newsletter_signup', 'nonce' ); ?>
```

### Retrieving Instance Storage

You're free to handle the user's form details however you want, but you'll need to call `$this->get_stored_instance( $storage_hash );`. The easiest way to handle a request would be to create an [AJAX handler](https://developer.wordpress.org/plugins/javascript/ajax/). Send the form to `admin-ajax.php` with `action` set to `my_newsletter_signup`. Visitors who aren't logged in need the `wp_ajax_nopriv_` hook.

```php
function my_newsletter_signup() {
    check_ajax_referer( 'my_newsletter_signup', 'nonce' );

    $hash = isset( $_POST['storage_hash'] ) ? sanitize_key( wp_unslash( $_POST['storage_hash'] ) ) : '';
    $email = isset( $_POST['email'] ) && is_string( $_POST['email'] ) ? sanitize_email( wp_unslash( $_POST['email'] ) ) : '';

    $widget = new My_Newsletter_Widget();
    $instance = $widget->get_stored_instance( $hash );

    // get_stored_instance() returns false for an unknown or expired hash.
    if ( empty( $instance['api_key'] ) || ! is_email( $email ) ) {
        wp_send_json_error();
    }

    // Now we can handle the rest of the input from $_POST.
    SomeNewsletterAPI::signup( $instance['api_key'], $email );

    wp_send_json_success();
}
add_action( 'wp_ajax_my_newsletter_signup', 'my_newsletter_signup' );
add_action( 'wp_ajax_nopriv_my_newsletter_signup', 'my_newsletter_signup' );
```

You're not limited to using instance storage in this specific way though. The Widgets Bundle stores the instance in a transient that expires seven days after the widget was last displayed, and it doesn't store the instance in a widget preview. Plan for `get_stored_instance()` to return `false` when the hash has expired.

To store only some of the instance, override the `modify_stored_instance()` method in your widget class. It receives the instance and returns the array to store. The storage hash is made from this array.
