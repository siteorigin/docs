# Hooks and Filters

The SiteOrigin Widgets Bundle gives you ample opportunity to extend and enhance the functionality of the bundle. We've already covered a lot of these filters in other sections of this documentation. This page just serves as a general reference.

## Actions

* [Version Update](actions/version-update.md) `'siteorigin_widgets_version_update'` - Triggered when the Widgets Bundle core is updated.
* [Initialize Widget](actions/initialize-widget.md) `'siteorigin_widgets_initialize_widget_' . $this->id_base` - Triggered after each `SiteOrigin_Widget` is initialized.
* [Enqueue Admin Scripts](actions/enqueue-admin-scripts.md) `'siteorigin_widgets_enqueue_admin_scripts_' . $this->id_base` - Gives widgets a chance to enqueue their scripts.
* [Enqueue Frontend Scripts](actions/enqueue-frontend-scripts.md) `'siteorigin_widgets_enqueue_frontend_scripts_' . $this->id_base` - Gives widgets a chance to enqueue their frontend scripts and styles.
* `'siteorigin_widgets_before_widget_' . $this->id_base` and `'siteorigin_widgets_after_widget_' . $this->id_base` - Triggered before and after the widget output. Receives `$instance` and `$widget`.

## Filters

* [Widget Folders](filters/widget-folders.md) `'siteorigin_widgets_widget_folders'` - Add folders where Widgets Bundle will look for widgets.
* [Active Widgets](filters/active-widgets.md) `'siteorigin_widgets_active_widgets'` - Filter which widgets are currently active.
* [Menu Capability](filters/admin-capability.md) `'siteorigin_widgets_admin_menu_capability'` - Change the capability required to enable/disable widgets.
* `'siteorigin_widgets_default_active'` - Filter the widgets that are active by default. The array keys are widget folder names.
* `'siteorigin_widgets_onclick_allowlist_functions'` - Filter the JavaScript functions allowed in onclick fields, such as the Button Widget's **Onclick** field.

### Widget Modifications

* [Form Options](filters/form-options.md) `'siteorigin_widgets_form_options'` and `'siteorigin_widgets_form_options_' . $this->id_base` - Modify the form array.
* [Frontend Widget Instance](filters/widget-instance.md) `'siteorigin_widgets_instance'` and `'siteorigin_widgets_instance_' . $this->id_base` - Filter the widget instance before it's rendered.
* [Form Widget Instance](filters/form-widget-instance.md) `'siteorigin_widgets_form_instance_' . $this->id_base` - Filter the widget instance before it's passed to the form renderer.
* [Template Variables](filters/template-variables.md) `'siteorigin_widgets_template_variables_' . $this->id_base` - Filter the variables passed to the template.
* [Template File](filters/template-file.md) `'siteorigin_widgets_template_file_' . $this->id_base` - Filter the template file path.
* [Template HTML](filters/template-html.md) `'siteorigin_widgets_template_html_' . $this->id_base` - Filter the raw template HTML.
* [Less File](filters/less-file.md) `'siteorigin_widgets_less_file_' . $this->id_base` - Filter the LESS file path.
* [Less Content](filters/less-content.md) `'siteorigin_widgets_less_' . $this->id_base` and `'siteorigin_widgets_less_vars_' . $this->id_base` - Filter the actual LESS content of the widget before it's processed.
* `'siteorigin_widgets_less_variables_' . $this->id_base` - Filter the LESS variables. Receives `$vars`, `$instance` and `$widget`.
* `'siteorigin_widgets_less_compiler'` - Filter the LESS compiler object, for example to register custom LESS functions. Receives `$compiler`, `$instance` and `$widget`.
* `'siteorigin_widgets_wrapper_classes_' . $this->id_base` - Filter the array of classes on the widget's wrapper `div`. Receives `$classes`, `$instance` and `$widget`.
* `'siteorigin_widgets_wrapper_id_' . $this->id_base` - Filter the `id` of the widget's wrapper `div`. Receives `$id`, `$instance` and `$widget`.
* [Widget CSS](filters/widget-css.md) `'siteorigin_widgets_instance_css'` - Filter the raw CSS generated from the LESS.
* [Sanitize Instance](filters/sanitize.md) `'siteorigin_widgets_sanitize_instance'` and `'siteorigin_widgets_sanitize_instance_' . $this->id_base` - Filter the instance before its stored in the database. This is designed to be used as a sanitization step.
