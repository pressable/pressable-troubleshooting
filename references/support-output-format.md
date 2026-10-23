# Support output format

Every output in this file has one target: the shortest thing that lets the reader act. The reader is a human, usually under time pressure, often reading in a chat window on a phone.

## Rules for anything the user will send onward

These apply to Support packets, developer handoffs, and vendor reports alike.

- **Lead with the ask.** The first line states what you want the reader to do or answer. Everything after it exists to support that ask.
- **Write it so the user can send it as their own message.** No preamble, no "I asked my AI assistant and it said," no restating the user's own situation back to them, no closing summary of what you just wrote.
- **Never paste the conversation.** Not the transcript, not your reasoning, not discarded hypotheses, not the path you took to get here. The reader needs the conclusion and the evidence under it.
- **Quote evidence, do not dump it.** Short relevant lines, each with its source and timestamp. Large logs, traces, and exports go in attached files referenced by name.
- **Length is a constraint, not an afterthought.** If a packet runs past roughly one screen, the usual cause is unknowns that were never narrowed. Narrow them, or state plainly that they could not be narrowed and why.
- **Let format follow content.** Lists, short labeled sections, and compact `test -> result` lines are usually clearer than paragraphs, and structure that carries meaning should stay. What costs the reader is structure that carries none: bullets nested three deep, headers inside headers, and the same fact restated in two places.
- **Redact before sending.** Strip passwords, API keys, tokens, session cookies, and customer personal data from anything you put in a packet, including log excerpts and screenshots. Replace with a placeholder and say what was removed.

## Omitting sections

Include a section when it carries information. Omit it only when investigation has positively established that it is not relevant to this issue, and say so in one clause where the reader would expect the section to be.

Never omit a section because it is unknown. An unknown is either:

1. eliminated before escalation, by performing the test that would answer it; or
2. stated explicitly as an open question under **Current assessment**.

A silently dropped unknown reads to the recipient as something that was checked and found unremarkable. That is the most expensive error this format can make. It sends Support down a path the user already knows is wrong, or stops anyone checking something nobody has checked.

## Diagnosis

### Result
One or two sentences stating the most supportable conclusion and confidence.

### Confirmed evidence
Only decisive observations, each labeled by source and environment.

### Hypotheses and unknowns
Separate plausible explanations from unresolved questions. State what would falsify each important hypothesis.

### Next steps
A short ordered list beginning with the safest, highest-information action.

## Investigation plan

For each proposed production write, state the status of the filesystem/site-files backup and database backup. If either is absent, include the explicit informed confirmation required by [safety and change management](safety-and-change-management.md) before presenting the action as approved or executing it.

For each step include:

1. action and environment;
2. evidence it collects;
3. expected interpretations;
4. risk and reversal, if any;
5. who should perform it.

## Developer or vendor handoff

- Exact product/component and version.
- Reproduction steps.
- Expected versus actual result.
- Decisive logs, trace, or error excerpts.
- Pressable constraints already verified.
- One specific requested investigation or change.

## Pressable Support evidence packet

Use this exact section order.

### Question for Pressable
One specific question or requested check.

### Site
- Domain
- Pressable site ID, if available
- Production, staging, sandbox, or clone
- Exact affected URL, endpoint, command, or operation

### Problem
One short paragraph describing expected versus actual behavior and business impact.

### Reproduction
A short numbered list with prerequisites, steps, expected result, actual result, and reproducibility rate.

### Failure window
Exact timestamp and timezone plus URL/action, method, source IP, user agent, and status when relevant.

### Confirmed evidence
Only decisive findings. Quote short relevant lines rather than entire logs. Label every site and environment.

### Tests completed
Compact `test → result` entries. Include who tested and where when relevant. Do not repeat disproven tests without explaining why.

### Current assessment
Separate confirmed findings, hypotheses, and unresolved questions. Never convert ambiguity into a platform diagnosis.

### Attachments or links
List untruncated logs, CSV exports, screenshots, backups, APM details/share links, and other raw evidence separately. Clearly label incomplete or truncated files.

## Example: a good Support packet

### Question for Pressable
Can you confirm whether Edge Cache is serving stale HTML for logged-out requests to /shop/ on example.com, and whether a purge was issued around 2026-09-14 14:20 PDT?

### Site
example.com, site ID 123456, production. Affected URL: https://example.com/shop/

### Problem
Product prices updated in wp-admin on 14 Sep are still showing the old values to logged-out visitors five days later. Logged-in admins see the correct prices. Customers are ordering at prices we no longer offer.

### Reproduction
1. Open https://example.com/shop/ in a private window.
2. Observe "Widget A - $19.00".
3. Log in as an administrator, reload the same URL.
4. Observe "Widget A - $24.00".

Reproducible every time, from three networks and two countries.

### Failure window
First observed 2026-09-14 ~14:20 PDT, continuous since. GET, no auth, various source IPs, HTTP 200.

### Confirmed evidence
- Response header on the logged-out request: `x-ac: 3.sea _atomic_bur HIT` (captured 2026-09-19 09:12 PDT).
- `server-timing: a8c-cdn, dc;desc=sea, cache;desc=HIT` on the same response.
- No matching request in origin NGINX logs for that window, consistent with an edge-served response.
- Cache purge from MyPressable on 18 Sep did not change the response.

### Tests completed
- Purge cache from MyPressable, 18 Sep 10:02 PDT -> no change; the next anonymous request still returned `x-ac: ... HIT` with the old price.
- Disable all caching plugins -> no change; none were active, only the platform-managed drop-ins are present.
- Query string bust (`/shop/?x=1`) -> correct prices returned, so the origin is generating correct HTML.

### Current assessment
Confirmed: the origin generates correct HTML, and the stale response is being served from edge. Unresolved: why the 18 Sep purge did not evict it. No evidence of an application-side cause, and we have not been able to test anything further from our side.

### Attachments or links
- `shop-headers-logged-out.txt`: full response headers for the stale request, captured 2026-09-19 09:12 PDT.
- `nginx-access-2026-09-14.csv`: MyPressable log export, 14:00 to 15:00 PDT, untruncated.

## Formatting constraints

The rules at the top of this file cover the ask, length, and evidence handling. These are additional:

- Preserve raw evidence without truncation in attached files, even where the packet quotes only a line or two of it.
- Use only evidence belonging to the user or safe public sources; never include another user’s case or confidential internal material.
- Never mix sites, environments, or timestamps without labels.
