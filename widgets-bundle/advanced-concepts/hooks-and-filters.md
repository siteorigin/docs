# Hooks and Filters

The Widgets Bundle's actions and filters let your theme or plugin change how widgets load, build their forms and render. Many of them have their own pages, linked below.

## Actions

- [Version Update Action](actions/version-update.md): `siteorigin_widgets_version_update` runs when the Widgets Bundle updates to a new version.
- [Initialize Widget Action](actions/initialize-widget.md): `siteorigin_widgets_initialize_widget_{$id_base}` runs after each `SiteOrigin_Widget` initializes.
- [Enqueue Admin Scripts Action](actions/enqueue-admin-scripts.md): `siteorigin_widgets_enqueue_admin_scripts_{$id_base}` lets a widget enqueue its admin scripts.
- [Enqueue Frontend Scripts Action](actions/enqueue-frontend-scripts.md): `siteorigin_widgets_enqueue_frontend_scripts_{$id_base}` lets a widget enqueue its front-end scripts and styles.
- `siteorigin_widgets_before_widget_{$id_base}` and `siteorigin_widgets_after_widget_{$id_base}` run before and after the widget's output, and receive `$instance` and `$widget`.

## Filters

- [Widget Folders Filter](filters/widget-folders.md): `siteorigin_widgets_widget_folders` adds folders where the Widgets Bundle looks for widgets.
- [Active Widgets Filter](filters/active-widgets.md): `siteorigin_widgets_active_widgets` changes which widgets are active.
- [Menu Capability Filter](filters/admin-capability.md): `siteorigin_widgets_admin_menu_capability` changes the capability that users need to activate and deactivate widgets.
- `siteorigin_widgets_default_active` changes which widgets are active on a new install. The array keys are widget folder names.
- `siteorigin_widgets_onclick_allowlist_functions` changes the JavaScript functions allowed in onclick fields, such as the Button Widget's **Onclick** field.

### Widget Filters

- [Form Options Filter](filters/form-options.md): `siteorigin_widgets_form_options` and `siteorigin_widgets_form_options_{$id_base}` change the form array.
- [Frontend Widget Instance Filter](filters/widget-instance.md): `siteorigin_widgets_instance` and `siteorigin_widgets_instance_{$id_base}` change the widget instance before the widget renders.
- [Form Widget Instance Filter](filters/form-widget-instance.md): `siteorigin_widgets_form_instance_{$id_base}` changes the widget instance before the form renders.
- [Template Variables Filter](filters/template-variables.md): `siteorigin_widgets_template_variables_{$id_base}` changes the variables passed to the template.
- [Template File Filter](filters/template-file.md): `siteorigin_widgets_template_file_{$id_base}` changes the template file's path.
- [Widget Template HTML Filter](filters/template-html.md): `siteorigin_widgets_template_html_{$id_base}` changes the template's raw HTML.
- [LESS File Filter](filters/less-file.md): `siteorigin_widgets_less_file_{$id_base}` changes the LESS file's path.
- [LESS Content Filter](filters/less-content.md): `siteorigin_widgets_less_{$id_base}` and `siteorigin_widgets_less_vars_{$id_base}` change the widget's LESS before the Widgets Bundle compiles it.
- `siteorigin_widgets_less_variables_{$id_base}` changes the LESS variables, and receives `$vars`, `$instance` and `$widget`.
- `siteorigin_widgets_less_compiler` changes the LESS compiler object, for example to register custom LESS functions, and receives `$compiler`, `$instance` and `$widget`.
- `siteorigin_widgets_wrapper_classes_{$id_base}` changes the classes of the widget's wrapper `div`, and receives `$classes`, `$instance` and `$widget`.
- `siteorigin_widgets_wrapper_id_{$id_base}` changes the `id` of the widget's wrapper `div`, and receives `$id`, `$instance` and `$widget`.
- [Widget CSS Filter](filters/widget-css.md): `siteorigin_widgets_instance_css` changes the CSS compiled from the LESS.
- [Sanitize Instance Filter](filters/sanitize.md): `siteorigin_widgets_sanitize_instance` and `siteorigin_widgets_sanitize_instance_{$id_base}` sanitize the instance before the Widgets Bundle saves it.

In each hook name, `{$id_base}` is the widget's base ID, such as `sow-button`.
