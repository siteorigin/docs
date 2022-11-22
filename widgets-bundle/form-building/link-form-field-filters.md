# Link Form Field Filters
The Link form field has two filters you can use to alter the results returned by the field.

### Filter: siteorigin_widgets_search_posts_results
This allows you to filter the results post array. This will allow you to selectively remove and sort filters as desired. 

```php
add_filter( 'siteorigin_widgets_search_posts_results' function( $results ) {
	foreach ( $results as $id => $result ) {
		// Your code here.
	}
	return $results;
} );
```

### Filter siteorigin_widgets_search_posts_order_by
This filter allows you to alter the ORDER BY query used by the links field. Default value is `post_modified DESC`.
```php
add_filter( 'siteorigin_widgets_search_posts_order_by', function( $ordered_by ) {
		return 'post_modified ASC';
} );
````
