## ⚙️ Changelog

### 1.0.12

- Refactor error message handling

### 1.0.11

- Maintenance update

### 1.0.10

- Added GitHub Actions output format when using ?ga parameter

### 1.0.9

- Add sites caching using transients to improve performance.

### 1.0.8

- Update documentation.

### 1.0.7

- Set the number of sites to 200. You can use the `add_filter( 'dss_cron_number_of_sites', function() { return 100; } );` to change the number of sites per request.

### 1.0.6

- Make plugin faster by using `$site->__get( 'siteurl' )` instead of `get_site_url( $site->blog_id )`. This prevents use of `switch_to_blog()` and `restore_current_blog()` functions. They are expensive and slow down the plugin.
- For `wp_remote_get`, set `blocking` to `false`. This will allow the request to be non-blocking and not wait for the response.
- For `wp_remote_get`, set `sslverify` to `false`. This will allow the request to be non-blocking and not wait for the response.

### 1.0.5

- Update composer.json with metadata

### 1.0.4

- Add namespace
- Tested up to WordPress 6.7
- Updated plugin description with license information.

### 1.0.3

- Fixed version compatibility

### 1.0.2

- Updated plugin description and tested up to version.

### 1.0.1

- Initial release.
