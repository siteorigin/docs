# Filtering the Widget Form

## Adding Content Before a Widget Form

The `siteorigin_panels_before_widget_form` action runs before Page Builder outputs a widget's form, so you can add your own HTML above the form. The action passes two arguments:

- `$the_widget`: the widget's `WP_Widget` object.
- `$instance`: the widget's Page Builder instance.

This example adds a note to the Archives widget form that recommends **Show post counts**:

```
function so_add_text_prior_to_archives_widget_form( $the_widget, $instance ) {
	if ( get_class( $the_widget ) == 'WP_Widget_Archives' ) {
		esc_html_e( 'We recommend ticking "Show posts count"', 'example-text-domain' );
	}
}
add_action( 'siteorigin_panels_before_widget_form', 'so_add_text_prior_to_archives_widget_form', 10, 2 );
```

## Replacing a Widget Form

The `siteorigin_panels_widget_form` filter changes a widget's form HTML before Page Builder outputs it, so you can change the form markup or replace the form with your own. The filter passes three arguments:

- `$form`: the form HTML from the widget.
- `$widget_class`: the widget's class name.
- `$instance`: the widget's Page Builder instance.

This example replaces the Calendar widget's form with a message:

```
function so_override_calendar_form( $form, $widget_class, $instance ) {
	if ( $widget_class == 'WP_Widget_Calendar' ) {
		return esc_html__( 'This is an example of the Calendar form being completely overridden.', 'example-text-domain' );
	}
	return $form;
}
add_filter( 'siteorigin_panels_widget_form', 'so_override_calendar_form', 10, 3 );
```
