# Page Builder Developer Docs

Page Builder has hooks, filters and theme support options for theme and plugin developers. Use them to add widgets and prebuilt layouts, change Page Builder's HTML and CSS output, and add your own row and widget settings. If you'd like to help improve Page Builder, read [Contributing to Page Builder](./page-builder/contributing.md).

## Widgets

Standard widgets work in Page Builder without changes, unless their JavaScript targets the WordPress Widgets screen. [Making Your Widgets Page Builder Compatible](./page-builder/widget-compatibility.md) explains the changes those widgets need.

You can also change how your widgets appear in the **Add Widget** dialog:

- [Placeholder Widgets](./page-builder/placeholder-widgets.md) add widgets your users haven't installed yet, so you can recommend widgets for them to install. You can also remove the widgets Page Builder recommends.
- [Page Builder Widget Groups](./page-builder/widget-groups.md) organize your widgets into groups.
- [Widget Icons](./page-builder/widget-icons.md) give your widgets their own icons.

If you build widgets, the Widgets Bundle gives you form fields, templates and LESS stylesheets to build on, as [Widgets Bundle Developer Docs](./widgets-bundle.md) explains.

## Prebuilt Layouts

A prebuilt layout is a complete page layout that your users insert with the **Layouts** button in Page Builder. [Page Builder Prebuilt Layouts](./page-builder/bundling-prebuilt.md) shows how to add layouts to your theme or plugin.

## Theme Integration

[Theme Integration](./page-builder/theme-integration.md) covers theme support, page templates, full-width rows and the style settings for rows, columns and widgets.

## Hooks and Filters

Page Builder's hooks and filters change its HTML and CSS output, its row and widget settings, its widget forms and the features of the builder itself.

- [Filtering Page Builder HTML Structure](./page-builder/hooks/html.md)
- [Page Builder CSS Hooks](./page-builder/hooks/css.md)
- [Filtering Custom Row Options](./page-builder/hooks/filtering-row-styles.md)
- [Filtering Widget Options](./page-builder/hooks/filtering-widget-styles.md)
- [Filtering the Widget Form](./page-builder/hooks/widget-form.md)
- [Filtering the Widget Instance](./page-builder/hooks/widget-instance.md)
- [Filtering Page Builder Features and Actions](./page-builder/hooks/builder-features-actions.md)
- [Overriding the Row Collapse Point](./page-builder/hooks/override-row-collapse-point.md)
- [Filtering the Row Form](./page-builder/hooks/row-form.md)
- [Stopping Row and Widget Output](./page-builder/hooks/stopping-output-of-row-widget.md)
