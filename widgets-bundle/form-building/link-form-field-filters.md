# Link Form Field Filters

The Link form field has three filters you can use to alter the results returned by the field.

### Filter: siteorigin_widgets_search_posts_post_types
This filter receives the array of post type names the field searches. By default, the field searches all public post types except attachments.

```php
add_filter( 'siteorigin_widgets_search_posts_post_types', function( $post_types ) {
	return array_diff( $post_types, array( 'product' ) );
} );
```

### Filter: siteorigin_widgets_search_posts_results
This allows you to filter the results post array. This will allow you to selectively remove and sort filters as desired. The field returns up to 20 published posts, and each result is an array with `value` (the post ID), `label` (the post title) and `type` (the post type). Return the array with `array_values()` so the field receives a list after you remove items.

```php
add_filter( 'siteorigin_widgets_search_posts_results', function( $results ) {
	foreach ( $results as $id => $result ) {
		// Your code here.
	}
	return array_values( $results );
} );
```

### Filter siteorigin_widgets_search_posts_order_by
This filter allows you to alter the ORDER BY query used by the links field. Default value is `post_modified DESC`. The returned value must be a valid ORDER BY clause for the `posts` table.
```php
add_filter( 'siteorigin_widgets_search_posts_order_by', function( $ordered_by ) {
		return 'post_modified ASC';
} );
````
