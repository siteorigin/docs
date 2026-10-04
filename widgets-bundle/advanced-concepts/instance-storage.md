# Instance Storage

Instance storage saves a widget's instance in your site's database and gives your template a hash that retrieves it. Use it when the front end needs to act on a widget setting without showing the setting in the page's HTML.

## An Example Use

A newsletter widget, for example, has a field for the user's Mailchimp API key. The widget needs the key only after a visitor submits the signup form, and the key must never appear in the page's HTML. The signup form sends the instance storage hash, and your code uses the hash to retrieve the full instance, including the API key, when the form arrives.

## Enabling Instance Storage

Set `instance_storage` to `true` in the widget's `$widget_options` array:

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
			array(),
			array(
				'api_key' => array(
					'type'  => 'text',
					'label' => __( 'API Key', 'siteorigin-docs' ),
				),
			),
			plugin_dir_path( __FILE__ )
		);
	}

	// Rest of the widget goes here
}
```

The Widgets Bundle then stores the widget's instance each time the widget renders.

## Using the `$storage_hash` Variable

With `instance_storage` enabled, your widget's templates have a `$storage_hash` variable. Add it to your form as a hidden field, or as a `data-storage-hash` attribute for an AJAX request:

```php
<input type="hidden" name="storage_hash" value="<?php echo esc_attr( $storage_hash ); ?>" />
<?php wp_nonce_field( 'my_newsletter_signup', 'nonce' ); ?>
```

## Retrieving the Instance

Call `$this->get_stored_instance( $storage_hash )` to retrieve the instance. An [AJAX handler](https://developer.wordpress.org/plugins/javascript/ajax/) is the most direct way to receive the form. Send the form to `admin-ajax.php` with `action` set to `my_newsletter_signup`. Hook the handler to `wp_ajax_nopriv_` as well, for visitors who aren't logged in:

```php
function my_newsletter_signup() {
	check_ajax_referer( 'my_newsletter_signup', 'nonce' );

	$hash  = isset( $_POST['storage_hash'] ) ? sanitize_key( wp_unslash( $_POST['storage_hash'] ) ) : '';
	$email = isset( $_POST['email'] ) && is_string( $_POST['email'] ) ? sanitize_email( wp_unslash( $_POST['email'] ) ) : '';

	$widget   = new My_Newsletter_Widget();
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

The Widgets Bundle stores the instance in a transient that expires seven days after the widget last rendered, and it doesn't store the instance in a widget preview. Handle `get_stored_instance()` returning `false` when the hash has expired.

`modify_stored_instance()` stores part of the instance. Override it in your widget class: it receives the instance and returns the array to store. The Widgets Bundle builds the storage hash from that array.
