# Security and access

Use this reference for Pressable MCP, account access, collaborators, SSH/SFTP, credentials, Jetpack Security, malware, and external-service troubleshooting.

## Pressable MCP

- Detect whether MCP is connected, authorized, and scoped to the intended account/site.
- Begin with supported read-only actions for site details, logs, metrics, backups, domains, plugins, themes, and collaborators. Before any MCP write against production, apply the production write gate in [safety and change management](safety-and-change-management.md).
- Do not assume arbitrary shell, database, edge-log, or server-administration access.
- Treat MCP tokens like passwords. Never ask users to paste an unredacted token into a chat, prompt, log, screenshot, or ticket.
- Separate Pressable MCP server behavior from the user's local client, operating system, runtime, package manager, shell, proxy, and custom-agent configuration.
- Contact Pressable when supported actions fail across clients or return incorrect Pressable data. Route local runtime/client faults to the party controlling that environment while preserving the option of limited Pressable guidance.

Sources: [Get Started with Pressable MCP](https://pressable.com/knowledgebase/get-started-with-pressable-mcp/), [Troubleshoot Pressable MCP with Your AI Assistant](https://pressable.com/knowledgebase/troubleshoot-pressable-mcp-with-your-ai-assistant/).

## Collaborators and credentials

- Give each developer an individual Pressable collaborator account with the minimum permissions needed.
- Never recommend sharing an owner account, another person's login, a generic team account, or an MCP token.
- Prefer staging access unless production access is necessary.
- Grant only required WordPress, database, SFTP/SSH, or account permissions.
- Remove or reduce access when the work ends to preserve individual attribution.
- Transfer temporary credentials only through a secure method, then rotate or revoke them.

Source: [Best Practices for Securing Account Ownership](https://pressable.com/knowledgebase/best-practices-for-securing-your-pressable-account-ownership/).

## Jetpack Security and suspected compromise

Confirm the relevant plugin, license, WordPress.com connection, scan, and backup states rather than treating plugin activation as proof every service is operational.

When compromise is suspected:

1. Preserve exact symptoms, first-observed times, relevant backups, logs, activity history, screenshots, users, and scan findings when safe.
2. Activate/connect included Jetpack Security when practical and check current documentation.
3. Inventory commercial plugins/themes before automated replacement because clean packages may require vendor access.
4. Contact Pressable Support for the documented malware-cleanup assistance.
5. Explain that cleanup is not formal forensics and may not establish entry point, actor, accessed data, or complete timeline.

Do not use WordPress core checksums as a meaningful integrity test on Pressable. Core is centrally managed, symlinked, and read-only, so a successful core checksum is expected and does not provide useful evidence that the writable site files or database are clean. Do not spend investigation time recommending core checksum verification unless a specific platform/core anomaly requires Pressable to investigate it.

Plugin checksum verification remains useful for plugins distributed through WordPress.org, but interpret mismatches in context rather than treating every difference as compromise. Pressable/WP Cloud uses managed, slightly modified builds of Jetpack and Akismet, so their files or version strings may legitimately differ from the public WordPress.org packages. Expected Jetpack or Akismet checksum differences—including WP Cloud-specific version suffixes—are not evidence of malware by themselves and must not trigger replacement, downgrade, or cleanup without independent evidence. Premium, custom, and vendor-distributed plugins may also lack usable WordPress.org checksums; compare those with a trusted package from the correct vendor instead.

A clean scan or successful plugin checksum result is not proof that all user files, database content, users, credentials, scheduled tasks, snippets, options, uploads, themes, or external accounts are safe. Focus investigation on writable content and persistence locations. Do not disable XML-RPC as generic hardening without checking Jetpack and other dependencies.

Sources: [Common Vectors for Malware and How to Mitigate Them](https://pressable.com/knowledgebase/common-vectors-for-malware-and-how-to-mitigate-them/), [What Is Jetpack Security?](https://pressable.com/knowledgebase/jetpack-security/), [Pressable Malware Cleanup](https://pressable.com/knowledgebase/pressable-malware-cleanup/).

## SSH, SFTP, WP-CLI, email, and external services

- Pressable provides SFTP, not unencrypted FTP.
- SSH is a constrained site-level application shell, not VPS/root access.
- Pressable WP-CLI behavior may differ from generic server examples.
- On WP Cloud, `wps` can be used instead of `wp --skip-plugins --skip-themes`. It provides some of the safeguards of those flags while still allowing plugin and theme code to execute, so treat it as a protected compatibility mode rather than proof that extensions were skipped.
- `--skip-plugins` does not necessarily skip must-use plugins.
- Database-only WP-CLI commands may avoid normal WordPress bootstrap and can be useful for read-only inventory when WordPress cannot load.
- Keep built-in transactional mail, third-party SMTP, mailbox hosting, DNS authentication, and external-provider behavior as separate layers.
- Check the actual outbound path and error before blaming DNS A records for API, webhook, SMTP, licensing, or external-service failures.
