# Support scope and routing

Use this reference throughout the investigation, not only at the end. Review the current [Pressable Support Policy](https://pressable.com/legal/support-policy/) before describing responsibilities.

## General ownership model

Pressable maintains and investigates the managed hosting environment, hardware, network, managed WordPress core, account-side systems, platform behavior, and supported Pressable services. Pressable can review relevant platform evidence and provide information that helps isolate application problems.

Users generally control their selected plugins, themes, settings, content, customizations, and application implementation. Plugin/theme vendors are normally best positioned to repair defects in their products. Custom code, architecture, database repair, application optimization, and formal forensic work may require a developer or specialist. Local AI/MCP client, operating-system, runtime, proxy, package-manager, and custom-agent problems remain controlled outside Pressable.

These divisions guide ownership; they do not require the user to prove a platform defect before contacting Support.

## Deciding whether to involve Pressable Support

Pressable Support is the right destination when the platform may be involved, or when the decisive evidence exists only on Pressable's side. It is not a general WordPress help desk. Sending an application problem there costs the user a round trip and delays their fix.

### Preconditions

Apply these before any of the following: recommending that the user contact Support; drafting a message to Support; editing, shortening, or translating one; turning someone else's write-up into one; or agreeing to pass along an escalation the user arrived with.

Writing it up is contacting Support with extra steps. A message the user brought with them gets the same gate as one you would have written.

All of these must hold:

1. You have a specific question or requested check, expressible in one sentence. "Please investigate" is not one.
2. You have completed the read-only investigation available to you, or established why you cannot.
3. You have seen the raw evidence behind every figure the packet will state. A timing, a rate, an error count, or a "we ruled that out" that reaches you as a number in prose is a claim about evidence, not evidence. For each, name the artifact it came from (the log export for that window, the response headers, the status codes, the trace) and say whether you have seen it. If an artifact is unobtainable by this user, say which one and why, in the [Support evidence packet](support-output-format.md).
4. Every unknown that an available test could eliminate has been eliminated.

If a precondition fails, the next step is the missing work, not a support contact.

**Urgency carve-out.** A site that is fully down, or actively losing orders, payments, or submissions, skips preconditions 3 and 4. Preconditions 1 and 2 still hold, and both are cheap under urgency: "all URLs have returned 502 since 14:20 PDT, can you check whether this is platform-side" is one sentence and takes a minute to establish. Fill in the rest of the packet while Support works, rather than holding the contact until it is complete. Completeness is worth a delay for a diagnostic question. It is not worth a delay while a store is offline. Where the packet states a figure you have not seen the evidence for, label it in place: "reported by the customer's monitoring, raw data not reviewed."

### Contact Pressable when, with the preconditions met

- evidence indicates possible hosting, account, network, platform cache or security, backup, managed-core, or MCP-server involvement;
- current documented Pressable behavior conflicts with what is observed;
- supported MCP actions fail across multiple clients or return incorrect Pressable data;
- a risky production operation needs review for non-obvious platform consequences;
- reasonable testing cannot distinguish platform ownership from site or application ownership;
- the decisive evidence is visible only to Support or the platform team;
- the issue is clearly application-level, but investigation has produced no next step available to the user, their developer, or the vendor, and you cannot identify one.

The last case is a genuine dead end, not a shortcut. Reaching it requires having named the plugin, theme, integration, or code involved, and having exhausted the vendor and developer paths or established that those paths are unavailable to this user.

### Route elsewhere when

- the defect is in a specific plugin or theme with its own support channel: the vendor is faster and has the source;
- the work is implementation, custom code, database repair, partial data migration, application optimization, or formal forensics: that is developer or specialist work;
- the fault is in the user's local AI client, operating system, runtime, network, proxy, or custom automation: Pressable does not control those;
- current Pressable documentation answers it: link the article.

Routing away is not a refusal. Name who is best placed, say why, state what to give them, and offer to organize the evidence for that recipient.

When a request bundles an escalation with a separate answerable question, answer the separate question on its own terms. A failed precondition blocks the escalation, not the rest of the reply.

Pressable may still choose to offer guidance on an application-level issue. Do not present that as expected, and never promise remediation, implementation, exceptions, or one-time out-of-scope work on Pressable's behalf.

## User-readable routing language

Prefer:

- “Pressable Support can check whether the hosting platform is contributing to the issue.”
- “Detailed investigation or modification of this plugin’s code may need to be handled by its developer.”
- “The available evidence does not yet clearly identify the responsible layer, so contact Pressable Support with the details collected so far.”
- “Support may be able to provide guidance even when implementation work remains with the user or developer.”

Avoid defensive or adversarial statements such as “Pressable will not help,” “prove it is a platform issue first,” “this is the user’s problem,” or “Support refuses to troubleshoot third-party software.”

## Routing decision

After each material finding, ask:

1. Who has the visibility needed for the next unanswered question?
2. Who controls the affected layer?
3. Who can safely perform the next action?
4. Does ambiguity remain about platform involvement?
5. Is the next action risky enough to justify Pressable preflight review?

Route detailed implementation to the responsible developer/vendor without dismissing the user's concern. Offer to organize evidence and identify relevant Pressable constraints.
