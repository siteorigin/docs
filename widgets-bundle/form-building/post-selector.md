# Post Selector

The post selector field lets users build a query that finds posts, for a widget that lists posts, such as a post carousel.

## Example

```php
$form_options = array(
	'some_posts' => array(
		'type'       => 'posts',
		'show_count' => true,
		'label'      => __( 'Some posts query', 'siteorigin-docs' ),
	),
);
```

The field saves the query as a string, such as `post_type=post&orderby=date&order=DESC&posts_per_page=3`. `siteorigin_widget_post_selector_process_query()` turns the string into an array that you pass to `WP_Query`. Its optional second argument, `$exclude_current`, is `true` unless you pass `false`, and adds the current post to `post__not_in`.

## Using the Query in a Template

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
		<?php
		while ( $query_result->have_posts() ) :
			$query_result->the_post();
			?>
			<li>
				<h3><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h3>
				<div>
					<?php $img = has_post_thumbnail() ? wp_get_attachment_image_src( get_post_thumbnail_id() ) : false; ?>
					<?php if ( ! empty( $img ) ) : ?>
						<a href="<?php the_permalink(); ?>" style="background-image: url(<?php echo sow_esc_url( $img[0] ); ?>)" aria-label="<?php the_title_attribute(); ?>"></a>
					<?php endif; ?>
				</div>
			</li>
			<?php
		endwhile;
		wp_reset_postdata();
		?>
	</ul>
</div>

<?php endif; ?>
```

## Filtering the Query

The `siteorigin_widgets_posts_selector_query` filter changes the array that `siteorigin_widget_post_selector_process_query()` returns. It changes the queries of SiteOrigin widgets that use the post selector, such as the Post Loop Widget, in ways the field's **Additional** setting can't. This example finds only posts whose `age` meta value is 3 or 4:

```php
<?php
add_filter(
	'siteorigin_widgets_posts_selector_query',
	function ( $query ) {
		$query['meta_query'] = array(
			array(
				'key'     => 'age',
				'value'   => array( 3, 4 ),
				'compare' => 'IN',
			),
		);

		return $query;
	}
);
```
