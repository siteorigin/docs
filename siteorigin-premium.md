# SiteOrigin Premium Developer Docs

SiteOrigin Premium is our way of adding features to all of our WordPress themes and plugins, while giving access to our premium email support.

## Removing the Teaser

We promote SiteOrigin Premium through small teasers in our plugins. If you're a theme developer building on top of Page Builder, Widgets Bundle or SiteOrigin CSS, we give you the option to hide the SiteOrigin Premium teasers.

This allows you to build on top of our ecosystem while monetizing users in the way you want to. Your users won't see the upgrade teasers.

```php
// Remove the SiteOrigin Premium teasers.
add_filter( 'siteorigin_premium_upgrade_teaser', '__return_false' );
```

Page Builder also has its own **Upgrade Teaser** setting at **Settings > Page Builder > General**. Turning it off hides the Page Builder teasers.

## Adding Your Affiliate ID

The `siteorigin_premium_affiliate_id` filter adds your affiliate ID to the SiteOrigin Premium links in our plugins as a `ref` query argument.

```php
function mytheme_premium_affiliate_id( $affiliate_id ) {
	return 'YOUR_AFFILIATE_ID';
}
add_filter( 'siteorigin_premium_affiliate_id', 'mytheme_premium_affiliate_id' );
```
