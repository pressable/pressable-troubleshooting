# Safety and change management

Use this reference before recommending or performing configuration changes, restores, deployments, deletions, database writes, plugin operations, domain changes, PHP-version changes, or security remediation.

## Production write gate

Production changes are not flatly prohibited. They may be appropriate when the evidence indicates that production is the necessary and safest target. However, before approving or performing any write-based action on production—including through Pressable MCP, the dashboard, SSH, SFTP, WP-CLI, the database, or the filesystem—apply this gate. Actions whose sole purpose is creating the required backups are exempt.

1. Strongly recommend a fresh full production backup, even when the change was already tested successfully on staging.
2. Explain that a full Pressable backup requires two separate recovery points: one filesystem/site-files backup and one database backup. Prefer fresh on-demand backups and confirm both completed successfully.
3. Record the backup timestamps, affected components, expected impact, validation checks, and rollback plan before execution.
4. If the user declines or cannot obtain either backup, stop before approving or executing the production write. Require a clear, affirmative confirmation that they are choosing to proceed without a complete backup and understand the increased risk of data loss, downtime, inconsistent files/database state, and limited rollback options. A generic earlier instruction to “go ahead” is not sufficient.
5. Explicit risk acceptance does not make an otherwise unauthorized, disproportionate, or unsupported action safe. Continue to prefer the narrowest reversible change and decline actions whose remaining risk is not supportable.

Source: [How to manually backup your site in Pressable](https://pressable.com/knowledgebase/how-to-manually-back-up-your-site/).

## Risk classes

### Low risk

Examples: read logs, inspect headers, list configuration/plugins/backups, check filesystem mode, capture a short APM trace.

Proceed when authorized. Explain any temporary overhead and preserve useful evidence.

### Moderate risk

Examples: purge cache, deactivate a plugin on staging, enable a short-lived diagnostic setting, create an on-demand backup, change non-production configuration.

State the expected effect, reversal method, validation check, affected environment, and observation window.

### High risk

Examples: production restores, database writes, file deletion, bulk plugin changes, staging-to-production pushes, primary-domain changes, PHP-version changes, collaborator permission changes, and malware cleanup.

Before recommending or using a high-risk action:

1. Confirm explicit user intent and authority.
2. Prefer validation on a current staging clone when feasible.
3. Complete the production write gate above, including fresh filesystem and database backups or the user’s explicit informed decision to proceed without a complete backup.
4. State whether files, database, or both will change.
5. Explain downtime, data-loss, cache, integration, and rollback risks.
6. Protect new orders, payments, users, submissions, comments, memberships, and other transactional changes.
7. Define success checks and rollback triggers before execution.
8. Recommend a Pressable Support preflight when platform consequences may be non-obvious.

Urgent recovery can justify direct production work, but staging remains the strong safety default rather than an absolute blocker. Urgency does not waive the production write gate; when backups cannot be obtained, explicit informed confirmation is still required before approval or execution.

## Backups, staging, cloning, and transfer

Do not use these terms interchangeably.

- A full backup requires separate filesystem/site-files and database backups, which may have different timestamps.
- Restoring one component can make references, media, code versions, options, and schemas inconsistent with the other.
- A clone creates another site state; it is not a restore guarantee.
- Staging-to-production data transfer can overwrite destination data rather than intelligently merge it.
- A week-old staging database can overwrite newer production orders, users, payments, or submissions.

Before production restoration or transfer, account for every change made after the source snapshot. Validate frontend, wp-admin, critical business workflows, files, database state, URLs, environment type, search-engine behavior, Jetpack state, integrations, and cache behavior afterward.

If the investigation indicates that only part of a WooCommerce database must move between sites, do not treat a normal staging-to-production transfer or full database replacement as a safe merge. Partial data migration is outside Pressable Support’s scope; Support may provide general guidance, but the user or a qualified developer/migration specialist must select, test, and perform the migration.

Use [Pressable’s WooCommerce partial-data migration guidance](https://pressable.com/knowledgebase/migrating-woocommerce-orders-and-customers-between-sites/) when forming the plan. Account for HPOS versus legacy order storage, related parent/child and lookup tables, extension- or gateway-specific tables, existing ID conflicts, and post-import integrity checks. The documented options include table-selective migration plugins, phpMyAdmin, WP-CLI, database command-line tools, and the WooCommerce REST API. Before proceeding, apply the production write gate and ensure both the source and destination databases have current backups.

Sources: [How to manually backup your site in Pressable](https://pressable.com/knowledgebase/how-to-manually-back-up-your-site/), [Staging Sites](https://pressable.com/knowledgebase/pushing-staging-to-production/), [How to Restore Your Website from a Backup](https://pressable.com/knowledgebase/restore-a-site-in-pressable-using-the-automated-restore-tool/), [WooCommerce partial-data migration guidance](https://pressable.com/knowledgebase/migrating-woocommerce-orders-and-customers-between-sites/).

## Production-write and destructive-tool guardrail

Tool availability is not authorization. Never run a write-based or destructive Pressable MCP, dashboard, SSH, WP-CLI, database, or filesystem action merely because it is technically available. Immediately before execution, reconfirm the target account, site, production environment, affected components, authorization, full-backup status, risk acknowledgment when applicable, and rollback plan.
