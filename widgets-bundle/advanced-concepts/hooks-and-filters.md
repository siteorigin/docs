# Hooks and filters

The SiteOrigin Widgets Bundle gives you ample opportunity to extend and enhance the functionality of the bundle. We've already covered a lot of these filters in other sections of this documentation. This page just serves as a general reference.

## Actions

* [Version Update](actions/version-update.md) `'siteorigin_widgets_version_update'` - Triggered when the Widgets Bundle core is updated.
* [Initialize Widget](actions/initialize-widget.md) `'siteorigin_widgets_initialize_widget_' . $this->id_base` - Triggered after each `SiteOrigin_Widget` is initialized.
* [Enqueue admin scripts](actions/enqueue-admin-scripts.md) `'siteorigin_widgets_enqueue_admin_scripts_' . $this->id_base` - Gives widgets a chance to enqueue their scripts.
* [Enqueue frontend scripts](actions/enqueue-frontend-scripts.md) `'siteorigin_widgets_enqueue_frontend_scripts_' . $this->id_base` - Gives widgets a chance to enqueue their frontend scripts and styles.

## Filters

* [Widget folders](filters/widget-folders.md) `'siteorigin_widgets_widget_folders'` - Add folders where Widgets Bundle will look for widgets.
* [Active widgets](filters/active-widgets.md) `'siteorigin_widgets_active_widgets'` - Filter which widgets are currently active.
* [Menu capability](filters/admin-capability.md) `'siteorigin_widgets_admin_menu_capability'` - Change the capability required to enable/disable widgets.

### Widget modifications

* [Form Options](filters/form-options.md) `'siteorigin_widgets_form_options_' . $this->id_base` - Modify the form array.
* [Frontend Widget Instance](filters/widget-instance.md) `'siteorigin_widgets_instance_' . $this->id_base` - Filter the widget instance before it's rendered.
* [Form Widget Instance](filters/form-widget-instance.md) `'siteorigin_widgets_form_instance_' . $this->id_base` - Filter the widget instance before it's passed to the form renderer.
* [Template Variables](filters/template-variables.md) `'siteorigin_widgets_template_variables_' . $this->id_base` - Filter the variables passed to the template.
* [Template File](filters/template-file.md) `'siteorigin_widgets_template_file_' . $this->id_base` - Filter the template file path.
* [Template HTML](filters/template-html.md) `'siteorigin_widgets_template_html_' . $this->id_base` - Filter the raw template HTML.
* [Less File](filters/less-file.md) `'siteorigin_widgets_less_file_' . $this->id_base` - Filter the LESS file path.
* [Less Content](filters/less-content.md) `'siteorigin_widgets_less_' . $this->id_base` - Filter the actual LESS content of the widget before it's processed.
* [Widget CSS](filters/widget-css.md) `'siteorigin_widgets_instance_css'` - Filter the raw CSS generated from the LESS.
* [Sanitize Instance](filters/sanitize.md) `'siteorigin_widgets_sanitize_instance_' . $this->id_base` - Filter the instance before its stored in the database. This is designed to be used as a sanitization step.
