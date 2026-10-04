# Link Form Field Filters

The link field searches your site's posts when a user looks for a page to link to. Three filters change that search.

## Post Types

The `siteorigin_widgets_search_posts_post_types` filter receives the array of post types the field searches. The field searches every public post type except attachments. This example removes products from the search:

```php
add_filter( 'siteorigin_widgets_search_posts_post_types', function( $post_types ) {
	return array_diff( $post_types, array( 'product' ) );
} );
```

## Results

The `siteorigin_widgets_search_posts_results` filter changes the search results, so you can remove or sort them. The field returns up to 20 published posts, and each result is an array with `value` (the post ID), `label` (the post title) and `type` (the post type). After you remove items, return the array through `array_values()` so the field receives a list:

```php
add_filter( 'siteorigin_widgets_search_posts_results', function( $results ) {
	foreach ( $results as $id => $result ) {
		// Your code here.
	}
	return array_values( $results );
} );
```

## Sort Order

The `siteorigin_widgets_search_posts_order_by` filter changes the `ORDER BY` clause of the search, which is `post_modified DESC` unless you change it. Return a valid `ORDER BY` clause for the `posts` table:

```php
add_filter( 'siteorigin_widgets_search_posts_order_by', function( $ordered_by ) {
		return 'post_modified ASC';
} );
```
