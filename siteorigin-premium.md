# SiteOrigin Premium Developer Docs

SiteOrigin Premium is our way of adding features to all of our WordPress themes and plugins, while giving access to our premium email support.

## Removing The Teaser

We promote SiteOrigin Premium through small teasers in our plugins. If you're a theme developer building on top of Page Builder, Widgets Bundle or SiteOrigin CSS, we give you the option to hide all references to SiteOrigin Premium.

This allows you to build on top of our ecosystem while monetizing users in the way you want to. Your users wont be made aware that there is a premium upgrade.

```
// Remove references to SiteOrigin Premium
add_filter( 'siteorigin_premium_upgrade_teaser', '__return_false' );
```