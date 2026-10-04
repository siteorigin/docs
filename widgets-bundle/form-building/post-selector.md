# Post Selector

The post selector field allows the user to build a query to find posts in the database. The resulting posts are then typically used in some form of list display, e.g., a post carousel.

## Example
Form options input:
```php
$form_options = array(
	'some_posts' => array(
		'type' => 'posts',
		'show_count' => true,
		'label' => __( 'Some posts query', 'siteorigin-docs' ),
	)
);
```

Use of the post selector will result in a pseudo query looking something like this:
`post_type=post&orderby=date&order=DESC&posts_per_page=3`.

This pseudo query may be transformed into a format understood by WordPress by using the `siteorigin_widget_post_selector_process_query()` function, which takes the pseudo query and an optional `$exclude_current` argument, and returns a query object which may be passed directly to the `WP_Query` constructor to find posts. `$exclude_current` defaults to `true`, which adds the current post to `post__not_in`.

### An Example Template Using the Post Selector Query

```php
<?php
$post_selector_pseudo_query = $instance['some_posts'];
// Process the post selector pseudo query.
$processed_query = siteorigin_widget_post_selector_process_query( $post_selector_pseudo_query );

// Use the processed post selector query to find posts.
$query_result = new WP_Query( $processed_query );

// Loop through the posts and do something with them.
if ( $query_result->have_posts() ) : ?>
<div>
	<ul>
		<?php while( $query_result->have_posts() ) : $query_result->the_post(); ?>
			<li>
				<h3><a href="<?php the_permalink() ?>"><?php the_title() ?></a></h3>
				<div>
					<?php $img = has_post_thumbnail() ? wp_get_attachment_image_src( get_post_thumbnail_id() ) : false; ?>
					<?php if ( ! empty( $img ) ) : ?>
						<a href="<?php the_permalink() ?>" style="background-image: url(<?php echo sow_esc_url( $img[0] ); ?>)" aria-label="<?php the_title_attribute(); ?>"></a>
					<?php endif; ?>
				</div>
			</li>
		<?php endwhile; wp_reset_postdata(); ?>
	</ul>
</div>

<?php endif; ?>
```

### Filtering Siteorigin_widget_post_selector_process_query

The `siteorigin_widgets_posts_selector_query` filter can be used to filter the array returned by `siteorigin_widget_post_selector_process_query`. This allows you to alter the query of SiteOrigin widgets (i.e., Post Loop) and widgets that function and make changes that otherwise wouldn't be possible with just the Additional field.

The following example will require results to have `age` meta with a value of `3` or `4`.

```php
<?php
add_filter( 'siteorigin_widgets_posts_selector_query', function( $query ) {
    $query['meta_query'] = array(
        array(
            'key'     => 'age',
            'value'   => array( 3, 4 ),
            'compare' => 'IN',
        ),
    );

    return $query;
} );
?>
```
