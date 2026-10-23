# Platform architecture

Use this reference for Pressable-specific filesystem, caching, server, domain, DNS, SSL, redirect, and network-address questions. Recheck linked documentation before relying on changeable details.

## Managed WordPress core and paths

Pressable manages WordPress core in a read-only, symlinked location separate from user-owned site content.

- Never recommend editing, deleting, replacing, downloading over, or changing permissions on WordPress core.
- Reject `wp core download --force` and broad recursive permission changes as routine repairs.
- Do not interpret managed-core paths in stack traces as duplicate installations or failed migrations.
- Verify that a suspicious core-looking filename belongs to the relevant WordPress version before classifying it.
- `ABSPATH` may resolve to managed core. Do not use `ABSPATH . 'wp-content/…'` to locate site content.
- Use `WP_CONTENT_DIR` for plugins, themes, uploads, and related content, correcting the path suffix rather than blindly replacing the constant.
- When software truly needs the physical writable site root, use `/srv/htdocs` or the documented portable-root pattern.

Sources: [Understanding ABSPATH at Pressable](https://pressable.com/knowledgebase/abspath-and-pressable/), [Using Advanced File Manager with Pressable](https://pressable.com/knowledgebase/using-advanced-file-manager-with-pressable/).

## PHP filesystem modes

Pressable provides PHP filesystem modes including read/write, read-only, and writes allowed only during authenticated WordPress sessions.

When any write, move, or create fails, and before any conventional `chmod`, ownership, or directory-permission advice:

1. Check the current PHP filesystem mode through Pressable MCP or MyPressable, and state which mode the site is in, or that it could not be determined, in your reply.
2. Identify whether the failing write occurs during wp-admin, cron, AJAX, REST, CLI, or an unauthenticated frontend request.
3. Resolve the actual target path.
4. Restrict any conventional permission repair to confirmed user-managed content.
5. Never recommend `777`.

A logged-in-only mode can explain why an admin action succeeds while cron, REST, or frontend background work fails. Current hacked-site-cleanup behavior and plugin exceptions are operational details: verify them rather than treating them as permanent guarantees.

Source: [Understanding PHP Filesystem Permissions](https://pressable.com/knowledgebase/understanding-php-filesystem-permissions/).

## Cache layers

Distinguish these layers:

- Batcache page caching through the platform-managed `advanced-cache.php` drop-in.
- Persistent object/query caching through the platform-managed `object-cache.php` and Memcache.
- Edge Cache for globally served pages and static assets.
- OPcache for compiled PHP bytecode.

Rules:

- Treat Pressable-owned `advanced-cache.php` and `object-cache.php` as expected read-only files. Never remove, replace, edit, or attribute them to a third-party plugin without direct ownership evidence.
- Do not routinely recommend third-party page or object caching. Evaluate minification, deferral, lazy loading, and image optimization separately.
- Avoid stacking multiple tools that transform the same assets without controlled testing.
- One cache `MISS` does not prove failure. Repeat anonymous requests after warm-up and account for login state, cookies, headers, query strings, recent purges, and dynamic routes.
- Edge-cached responses avoid PHP workers; object-cached requests still execute PHP.
- Base OPcache for compiled PHP bytecode is enabled by default for managed WordPress core and platform files on all Pressable sites.
- User File OPcache covers plugins, themes, and other user-managed PHP files. It is a separate per-site setting and must not be recommended as a generic response to slowness or high TTFB.
- Before recommending User File OPcache, perform or review a performance investigation that provides a supportable reason to expect reduced PHP compilation or execution overhead. Verify its current effective state, identify the measured bottleneck, and define how the expected benefit will be validated. If the investigation does not establish likely value, do not recommend enablement.
- Do not recommend restarting, rebooting, or resetting OPcache as a troubleshooting or remediation step. Pressable Support cannot restart the managed OPcache/PHP service, so do not present a restart as an available Support action.

Source: [Caching Types Available in Pressable](https://pressable.com/knowledgebase/caching-types-available-in-pressable/).

## Fixed managed-server configuration

Pressable uses managed NGINX rather than user-configurable Apache.

- Do not recommend `.htaccess`, custom NGINX rules, or unsupported `php.ini`/`.user.ini` changes for documented fixed values.
- Find a documented WordPress- or Pressable-level alternative.
- `custom-redirects.php` can run early PHP redirects, headers, or request handling before Batcache, but it is not NGINX configuration.
- Existing static files are normally served before PHP. Lightweight 404 behavior may also prevent missing static-file requests from reaching PHP.

Sources: [PHP Info and Limitations](https://pressable.com/knowledgebase/php-info-and-limitations-on-pressable/), [Adding custom redirects](https://pressable.com/knowledgebase/custom-redirects/).

## Domains, DNS, SSL, and redirects

Keep these separate: domain assignment, DNS resolution, proxying, SSL provisioning, WordPress Home/Site URLs, primary-domain selection, canonicalization, and application redirects.

- Non-primary domains normally canonicalize to the primary domain before PHP, so application code may never receive the original hostname.
- Prefer changing the primary domain through MyPressable; do not default to manual `wp_options`, `WP_HOME`, or `WP_SITEURL` changes.
- Pressable SSL depends on correct domain assignment and DNS. Review current guidance before suggesting Cloudflare modes, AAAA removal, CAA changes, or certificate retries.
- Account-association problems can require verification or Support rather than more DNS waiting.
- Verify current plan/network restrictions before promising multisite or nested-subdomain behavior.

Sources: [How to Point Your Domain to Pressable](https://pressable.com/knowledgebase/how-to-point-your-domain-to-pressable/), [Solutions to Common SSL Issues](https://pressable.com/knowledgebase/solutions-to-common-ssl-issues/).

## Inbound and outbound addresses

### Inbound A-record ranges

Pressable inbound addresses fall within `199.16.172.1-199.16.172.254` and `199.16.173.1-199.16.173.254`. These have been stable for years and can be relied on when present.

They are not definitive in either direction:

- An address in these ranges is strong evidence the domain points at Pressable. It does not establish that the domain is assigned to a site here; a domain pointed at Pressable but not attached to a site returns `Domain not found`.
- An address outside them does not rule Pressable out. Sites commonly resolve to Cloudflare, Sucuri, SiteLock, or another proxy in public DNS and route to Pressable behind it. Use the response-header check in the main workflow when DNS is ambiguous.


Pressable A-record addresses are inbound Anycast destinations. They do not identify the address used for outbound API, webhook, SMTP, or vendor traffic.

- Use the documented SSH or temporary-PHP method to observe the current outbound address when needed.
- Delete temporary public diagnostic files immediately.
- Never describe an observed outbound address as fixed, dedicated, permanent, or guaranteed.
- Prefer Pressable's complete current outbound CIDR ranges when an external service supports ranges; obtain current ranges through Support rather than embedding them.
- Contact Support when a vendor accepts only one fixed address.

Source: [Understanding Outbound IP Addresses](https://pressable.com/knowledgebase/understanding-outbound-ip-addresses-on-pressable/).
