# Page Builder Developer Docs

We built Page Builder to work with just about any WordPress theme, but there are lots of things you can do to customize the experience for your users. Please read our [Contributing to Page Builder](./page-builder/contributing.md) guide if you'd like to help improve Page Builder.

## Widgets

Page Builder supports most standard widgets. There are a few things that you should be aware of though. Especially related to how your widget should handle Javascript. Read our [compatibility guide](./page-builder/widget-compatibility.md) on making your widgets work with Page Builder.

There are a few ways that you can either register widgets that only appear in widgets area or Page Builder.

Page Builder has a few unique widget related concepts.

* [Placeholder widgets](./page-builder/placeholder-widgets.md) let you add widgets that don't yet exist to the Add Widget dialog. This feature is useful for recommending widgets that you might want your user to install. You can also remove the widgets that Page Builder itself recommends.
* [Widget groups](./page-builder/widget-groups.md) let you organize your widgets within the Add Widget dialog.
* [Widget icons](./page-builder/widget-icons.md) let you make your widgets stand out in the Add Widget dialog.

For widget developers, you could consider using the [SiteOrigin Widgets Bundle](./widgets-bundle.md) as a boilerplate for your widgets. It gives you a base to create some fairly advanced widgets and widget interfaces with minimal coding.

## Prebuilt Layouts

Prebuilt Layouts are a great way to give your users some designs that they can use with your theme. A prebuilt layout is essentially a complete layout that your users can insert. Read our guide on [bundling prebuilt layouts](./page-builder/bundling-prebuilt.md) for your pages.

## Hooks and Filters

Page Builder has lots of hooks and filters for you to use. We've already covered these briefly in the Widgets and Prebuilt Layouts sections, but there are other filters you can use to inject custom content, filter output, etc.

* [Filtering HTML Structure](./page-builder/hooks/html.md).
* [Filtering CSS](./page-builder/hooks/css.md).
* [Filtering Custom Row Options](./page-builder/hooks/custom-row-settings.md).
- [Filtering Page Builder Features and Actions](./page-builder/hooks/builder-features-actions.md).
