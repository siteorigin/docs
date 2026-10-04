# SiteOrigin Premium Developer Docs

SiteOrigin Premium extends our plugins and themes with addons, and includes email support from our team.

## Removing the Teasers

Page Builder, the Widgets Bundle and SiteOrigin CSS show small teasers that promote SiteOrigin Premium. Theme developers who build on our plugins can hide the teasers with the `siteorigin_premium_upgrade_teaser` filter:

```php
// Remove the SiteOrigin Premium teasers.
add_filter( 'siteorigin_premium_upgrade_teaser', '__return_false' );
```

Disable **Upgrade Teaser** at **Settings > Page Builder > General** to hide only the Page Builder teasers.

## Adding Your Affiliate ID

The `siteorigin_premium_affiliate_id` filter adds your affiliate ID as a `ref` query argument to the SiteOrigin Premium links in Page Builder and in the Widgets Bundle's widget forms.

```php
function mytheme_premium_affiliate_id( $affiliate_id ) {
	return 'YOUR_AFFILIATE_ID';
}
add_filter( 'siteorigin_premium_affiliate_id', 'mytheme_premium_affiliate_id' );
```
