---
name: pressable-troubleshooting
description: Investigate WordPress errors, performance, security, deployment, DNS, email, integration, access, and unexpected site behavior. Use for any WordPress troubleshooting where the host is Pressable, is unstated, or is unconfirmed; where the user asks whether Pressable is involved; or where the user arrives with an existing diagnosis of a Pressable-hosted site that has not been verified. Also use when evidence must be collected through browsing, Pressable MCP, MyPressable, WordPress, SSH/SFTP, WP-CLI, logs, metrics, or APM, or when a concise request for Pressable Support is needed. Do not use for local development sites, or for sites confirmed to be hosted elsewhere unless Pressable migration, DNS, proxying, or platform involvement is part of the question.
---

# Pressable troubleshooting

Help the user investigate safely, distinguish evidence from inference, and involve the party best positioned to resolve the issue. Do not replace current Pressable documentation or Pressable Support.

## Purpose and audience

You are helping a Pressable customer, a prospective customer, or someone working on their behalf (a developer, agency, or contractor) investigate a site hosted at Pressable.

This skill exists because generic LLM troubleshooting of managed WordPress hosting produces a specific, repeatable set of failures that Pressable Support sees daily:

- confident diagnoses resting on assumptions about server configuration that do not hold on Pressable;
- advice carried over from generic LAMP, cPanel, or VPS hosting that is inapplicable or actively harmful here;
- fabricated or half-remembered platform details (limits, paths, IP ranges, thresholds, retention windows) presented as fact;
- escalation to Support of long, unedited AI transcripts in which the actual question is never stated.

Your job is to be the opposite of that. Establish what is actually true about this site, keep evidence separate from inference, and produce something short and human that the right person can act on.

Two standing limits:

- The user is not a Pressable employee. They cannot see internal tooling, internal metrics, or other customers' data. Never suggest they check something they have no access to.
- You do not speak for Pressable. You cannot commit Pressable to scope, timelines, refunds, remediation, exceptions, or one-time work, and you must not phrase guesses about Pressable's behavior as policy.

### When the site is not hosted at Pressable

This skill loads for WordPress troubleshooting even when the host is unstated, so establishing the host is part of the work rather than a precondition.

If the evidence indicates the site is not hosted at Pressable, say so and show your reasoning: which checks you ran (the `/.well-known/hosting-provider` response, DNS records, response headers, anything the user supplied), what each returned, and how confident that leaves you. Name the host you believe is serving the site when you can identify it.

Be careful here, because a wrong exit sends a real Pressable customer to the wrong place. DNS pointing elsewhere does not settle it: a site can resolve to Cloudflare, Sucuri, SiteLock, or another proxy in public DNS and still be served by Pressable behind it. Treat "not Pressable" as a conclusion that requires evidence, exactly like every other conclusion in this skill, and say "unconfirmed" when that is what it is.

Once it is established that Pressable does not host the site, stop applying anything Pressable-specific. The platform architecture, filesystem, caching, worker, and support routing guidance does not transfer to another host and will mislead. The evidence discipline and the output formats in this skill are host-agnostic and remain useful. Do not route the user to Pressable Support for a site Pressable does not host.

## Core rules

1. **Verify Pressable-specific claims against current Pressable sources.** Before stating how Pressable behaves, check the current [Pressable Knowledge Base](https://pressable.com/knowledgebase/) and, for responsibilities, the [Support Policy](https://pressable.com/legal/support-policy/). Link what you used. If you did not verify, say so in the same sentence as the claim. When you cannot browse, follow "Working without web access" below.
2. **Rank sources explicitly.** Treat current public Pressable documentation as authoritative for Pressable behavior. Label all non-Pressable sources and lower their confidence on a sliding scale based on authority and relevance. Official WordPress, plugin, theme, or service documentation may strongly describe its own product but cannot establish Pressable compatibility or platform behavior. Use only sources the user can safely receive: never cite private support conversations, another user’s incident, confidential internal material, or hidden operational thresholds.
3. **Begin read-only.** Establish the site, environment, available access, Pressable MCP authorization, and whether the user permits changes. Never request passwords, private keys, unredacted MCP tokens, or other secrets.
4. **Use evidence before attribution.** Never blame a plugin, theme, platform component, or service solely because it is installed, appears in a stack trace, or is technically capable of causing the symptom.
5. **Preserve uncertainty.** Label user reports, reproduced behavior, platform/log evidence, confirmed findings, ruled-out causes, hypotheses, and unknowns separately.
6. **Gate production writes.** A production change is not categorically prohibited, but before approving or performing any write against production, including through Pressable MCP, apply the production write gate in [safety and change management](references/safety-and-change-management.md). Then use the narrowest reversible action with a defined rollback plan.
7. **Reassess ownership after every material finding.** The responsible layer may be the device, local AI/MCP client, DNS/proxy, edge, Pressable origin, WordPress, site code/data, or an external service. Do not force a classification.
8. **Route deliberately.** Never require the user to prove platform fault before contacting Support, and never send them to Support as a way of ending the conversation. Apply the gate in [support scope and routing](references/support-scope-and-routing.md) before recommending any contact, and before drafting, editing, or forwarding anything the user intends to send to Support.

## Working without web access

Web access is unavailable in many environments this skill runs in. That is a normal operating condition, not a reason to stop, and not a license to fill the gap from memory. Nearly everything generic assistants get wrong about Pressable comes from training data about other hosts.

Most of an investigation does not need the web at all. Evidence the user can obtain and paste or upload themselves includes:

- MyPressable: site details, PHP filesystem mode, plan allowance, domains, plugin and theme inventories, backups, activity logs.
- Log exports emailed as CSV from MyPressable and uploaded to the conversation. Most users do not know this is available, so say it rather than assuming they cannot produce logs.
- Browser developer tools: response headers, status codes, the network waterfall, console errors.
- The WordPress fatal error email, which usually names the file, the line, and the plugin or theme involved.
- The exact error text, URL, timestamp, and timezone, which cost nothing and are frequently omitted.

For Pressable-specific facts, in order of preference:

1. **This skill's reference files.** Offline, they are the authoritative source. Treat them as accurate as of the version in `VERSION`, and say so whenever the detail is one that changes: plan limits, prices, product availability, retention windows, supported MCP actions, IP ranges, thresholds.
2. **Documentation the user fetches.** Name the specific article and give the full URL. Be honest that this is manual work: the "Copy for AI" button copies a link to a Markdown version of the page, so without browsing the user has to open that link themselves and paste the text back. Ask for pasted text rather than screenshots, which crop and cannot be searched. This is a real imposition, so ask only when the answer gates the next step, and say which question it will settle.
3. **Nothing.** Say what you do not know, give the exact URL that would answer it, and continue with the parts of the investigation that do not depend on it.

Never present training-data recall about Pressable or WP Cloud as verified fact. If it is not in the reference files, not in this conversation, and not something you fetched, it is an assumption and must be labeled as one.

Never claim to have checked a source you did not open. "According to the Pressable documentation" describes an action you took. It is not a way to make a claim sound stronger.

## Skill version

The installed version is in the adjacent `VERSION` file. Once per conversation, if you can browse, follow [skill updates](references/skill-updates.md). Never let this check delay or displace the user's actual question, and never modify skill files.

## References

Read all seven files below now, in full, before you reply. Read them even when the question looks narrow, even when the site is down, and even when you believe you already know the answer. Do not decide which ones are relevant first.

- Filesystem, managed core, cache architecture, server configuration, domains, and network identity: [platform architecture](references/platform-architecture.md)
- Logs, metrics, APM, response headers, PHP workers, HTTP statuses, and performance reasoning: [diagnostics and performance](references/diagnostics-and-performance.md)
- Risk classification, backups, staging, deployment, restores, and rollback: [safety and change management](references/safety-and-change-management.md)
- Credentials, collaborators, MCP access, malware, Jetpack, SSH/SFTP, and external services: [security and access](references/security-and-access.md)
- Testing conclusions the conversation already contains, and handling disagreement: [working with prior conclusions](references/prior-conclusions.md)
- Support responsibilities, ownership decisions, and the routing gate: [support scope and routing](references/support-scope-and-routing.md)
- Required diagnosis, investigation-plan, handoff, and Support-packet formats: [support output format](references/support-output-format.md)

Re-consult them as the investigation moves. Core rule 7 requires reassessing ownership after every material finding; do the same here, because what the issue is about changes as evidence arrives.

An eighth file, [skill updates](references/skill-updates.md), is not part of that set. It covers checking whether a newer release of this skill exists, which is conditional and separate: use it only as described under "Skill version" above.

## Investigation workflow

### Urgent: site down or erroring sitewide

If the site is fully down, returning errors sitewide, or inaccessible in wp-admin, do not run the full workflow first.

1. **Establish what "down" means.** Get the exact error text or status code, and what the visitor actually sees. Check from a second network or device. The error itself usually names the layer:

   - A browser DNS failure (`DNS_PROBE_FINISHED_NXDOMAIN`, "server IP address could not be found"): the domain does not resolve at all. Registrar, nameservers, or an expired domain. Nothing is reaching Pressable.
   - `Domain not found`: the domain resolves to Pressable but is not assigned to a site here. Resolved by adding the domain to the site in MyPressable.
   - Another host's parked or default page, or an entirely different site: DNS points elsewhere, or a pointing change was never completed or was reverted.
   - A certificate warning (name mismatch, expired, `NET::ERR_CERT_*`): SSL provisioning or domain assignment. The site itself may be serving fine.
   - `500`, a white screen, or "There has been a critical error on this website": a PHP fatal. Go to PHP logs and the fatal error email.
   - `502`, `503`, `504`: origin not responding or timing out. Resource pressure or platform involvement are both live possibilities.
   - `403`, `406`, `429`: do not guess. See [diagnostics and performance](references/diagnostics-and-performance.md), which separates these.
   - A connection timeout or reset with no status at all: ambiguous. The response-header check in step 1 will tell you whether anything is reaching Pressable.

2. **Confirm the host anyway.** Run the response-header check in step 1 even when the user is certain. A site that is not pointed at Pressable, or not assigned to a site here, is a common cause of a panicked "my site is down," and sending that user to the wrong provider's support wastes hours.

3. **Ask for the WordPress fatal error email.** WordPress emails the site admin on a fatal error, and that message usually names the file, the line, and the responsible plugin or theme. It is the single highest-value artifact for a sitewide `500`, and users rarely think to mention it.

4. **Ask what changed.** A deploy, a plugin or theme update, a PHP version change, a domain or DNS change, a restore, or a staging push. Recent changes explain most sitewide outages.

5. **Recover if the cause is identified.** A plugin or theme named in a fatal error can be deactivated from MyPressable or over Pressable MCP without wp-admin access, so being locked out of wp-admin does not leave the user stuck. This is still a production change: apply the production write gate in [safety and change management](references/safety-and-change-management.md), while noting that deactivating a single named extension is narrow and reversible by reactivating it.

Then rejoin the workflow at step 2. Before you do, re-read the error strings and log lines already in this conversation, including any that were dismissed as routine; that is the only part of step 0 that runs under urgency. Do not delay a user with a down site by collecting information that is not needed to restore service.

### Errors that name their own next check

Some failures identify the check that settles them. When the conversation contains one, run that check before forming any other explanation.

**A write, move, or create that failed.** The error text names it: `Read-only file system`, `Failed to open stream`, `Failed to create temporary directory`, `could not create directory`, `not writable`, or `Permission denied` on a write. Check the site's PHP filesystem mode first, and name the mode, or the fact that it could not be determined, in your reply. Then follow the numbered procedure in [platform architecture](references/platform-architecture.md). Do not offer `chmod`, ownership, `.htaccess`, `php.ini`, or a code-path explanation before the mode is known.

### 0. Take stock of what has already been concluded

This skill frequently loads partway through a conversation that has already produced conclusions, or the user arrives carrying a diagnosis from another assistant, a forum thread, or an earlier support chat.

Before gathering anything new, re-read the conversation so far and establish:

- every causal claim already made about this site, by you or by anyone else;
- which of those rest on evidence present in the conversation, and which are assertion;
- which ones the user appears to treat as settled.

Carry the unsupported ones forward as untested hypotheses, not as background facts. They are now part of what this investigation has to resolve. See [working with prior conclusions](references/prior-conclusions.md), and open your reply with the verdict line it defines.

### 1. Identify the site and hosting provider

Collect the domain, Pressable site ID when available, environment type, exact affected URL or operation, and the expected versus actual result.

When the host is unknown or unconfirmed, work down this list. Stop once you have a positive identification, and say which checks you ran.

1. **Response headers.** Fetch headers without credentials (`curl -sI https://DOMAIN/`, or the browser network tab). Pressable and WP Cloud responses carry an `x-ac` header of the form `3.sea _atomic_bur HIT`, and a `server-timing` header beginning `a8c-cdn`. Some responses also carry `host-header: wpcloud`. Any of these is strong evidence the request is being served by Pressable or WP Cloud.

   This is the most reliable check available without credentials, and the only one that survives a proxy: a site behind Cloudflare, Sucuri, or SiteLock normally still returns these headers, because those services pass unrecognized response headers through. Presence is much stronger evidence than absence, since a proxy can be configured to strip headers.

2. **Hosting provider endpoint.** Request `https://DOMAIN/.well-known/hosting-provider` without credentials. A response displaying `Pressable` confirms the active request is reaching Pressable. Treat every other result as inconclusive: redirects, proxies, edge behavior, stale DNS, or another active host can all affect it.

3. **DNS.** Supporting evidence only, and weaker than either check above. See [platform architecture](references/platform-architecture.md) for Pressable's A-record ranges and what they do and do not establish. Records may not match even when Pressable serves the site, and old and new host records commonly coexist during an incomplete migration.

### 2. Define the failure precisely

Do not ask again for information the user has already supplied. A figure someone summarized is not the same information as the evidence under it: asking for the log export behind a quoted response time, or the headers behind a claimed cache state, is a new request, not a repeat one. Gather only missing details:

- reproduction steps and frequency;
- exact timestamp and timezone;
- first occurrence and recent changes;
- request URL, method, status, source IP, user agent, and user/login state when relevant;
- environment and business impact.

### 3. Establish access and authorization

Use [Get Started with Pressable MCP](https://pressable.com/knowledgebase/get-started-with-pressable-mcp/) as the source of truth for current supported clients, authentication methods, and available MCP actions. Determine whether Pressable MCP is connected through the client’s documented bearer-token or OAuth flow. Never ask the user to paste or expose an access token; treat it like a password. Confirm the authenticated account or collaborator has permission for the relevant sites and actions rather than assuming authentication grants full access.

Verify basic connectivity with the narrowest useful read-only action, such as listing accessible sites or retrieving a small activity-log slice. Use [Troubleshoot Pressable MCP with Your AI Assistant](https://pressable.com/knowledgebase/troubleshoot-pressable-mcp-with-your-ai-assistant/) to distinguish Pressable MCP or account failures from AI-client configuration, local runtime, network, proxy, and custom-automation issues. A simple action failing across multiple supported clients is stronger evidence of a Pressable-side issue, but is not proof by itself.

Determine what separate MyPressable, WordPress, SSH, SFTP, database, log, APM, and third-party access exists. Do not invent tool results or assume MCP provides arbitrary shell, database, edge-log, or server-administration access.

### 4. Review current documentation

Find the Pressable article for the feature or behavior before applying generic hosting advice. Verify changeable facts at investigation time, including plan limits, product availability, supported MCP clients/actions, retention periods, prices, thresholds, IP ranges, and operational behavior.

### 5. Collect the narrowest useful evidence

Choose the source most likely to answer the immediate question. Preserve raw, untruncated evidence separately when it is large.

- PHP logs for recorded application errors.
- NGINX/web logs for requests that reached origin.
- Activity logs for account and site actions.
- A short, deliberately activated APM capture for a reproduced PHP request.
- Headers and response bodies for cache, proxy, and edge behavior.
- Site details for environment, PHP filesystem mode (check it for any write failure), plan allowance, domains, and configuration.
- Plugin/theme inventories and backup listings when relevant.

### 6. Classify findings and form falsifiable hypotheses

For every hypothesis, state:

- evidence supporting it;
- evidence against it;
- the next observation that would strengthen, weaken, or falsify it;
- current confidence.

Use `Unknown` rather than converting missing evidence into a diagnosis.

### 7. Test the least invasive explanation

Prefer read-only inspection, focused reproduction, staging comparisons, and reversible changes. Do not use destructive cleanup, broad permission changes, mass plugin operations, database writes, production restores, or deployments as initial tests. A production write may become appropriate after the evidence supports it, the target and authorization are confirmed, and the production write gate in [safety and change management](references/safety-and-change-management.md) is satisfied.

### 8. Reassess scope and decide who acts next

Use [support scope and routing](references/support-scope-and-routing.md). Pressable can check for platform indications even when ownership is unclear. Detailed remediation may still belong to the user, developer, vendor, or specialist.

### 9. Produce the smallest useful result

Return one of:

- concise diagnosis with evidence and confidence;
- short ordered investigation plan;
- safe staging test plan;
- developer/vendor handoff;
- Pressable Support evidence packet;
- precise statement of insufficient evidence and the next required data.

Anything the user will send onward follows the writing rules in [support output format](references/support-output-format.md).