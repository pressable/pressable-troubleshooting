# Pressable Troubleshooting

An AI assistant skill for investigating WordPress sites hosted at [Pressable](https://pressable.com).

## Why This Exists

AI assistants know WordPress well and sound confident about hosting. That combination goes wrong on a managed platform, because most of what an assistant has learned about servers comes from generic LAMP, cPanel, and VPS setups that do not describe Pressable. The result is advice that reads as authoritative and is wrong: permission fixes for a filesystem that does not work that way, edits to a WordPress core that is read-only, `.htaccess` rules on a server that never reads them, and diagnoses resting on limits and thresholds the assistant invented.

This skill constrains that. It gives your assistant accurate information about how Pressable actually works, plus a method for keeping what has been observed separate from what has been assumed. Where it does not know something, it is instructed to say so rather than fill the gap.

## What It Changes

With the skill loaded, your assistant should:

- check current Pressable documentation before making claims about Pressable behavior, and tell you when it could not;
- keep evidence, hypotheses, and unknowns separate, rather than presenting a guess as a diagnosis;
- test conclusions instead of asserting them, including conclusions reached earlier in the same conversation, by it or by you;
- recommend a backup and explain the risk before any change to a production site;
- tell you who is best placed to help, which is sometimes Pressable Support and sometimes a plugin vendor or a developer;
- produce a short, specific summary you can send to Pressable Support, rather than a transcript you have to paste.

That last point matters to both sides. A support conversation that opens with the actual question, the evidence, and what has already been ruled out gets resolved considerably faster than one that opens with several screens of AI output.

When the conversation already contains a diagnosis, whether from you, another assistant, a forum thread, or earlier in the same chat, the assistant opens its reply with a one-line verdict on it:

```text
Prior conclusion: not yet tested. What would settle it: check the site's PHP filesystem mode in MyPressable.
```

The verdict is *holds up*, *does not fit*, or *not yet tested*, and the second half names the quickest check that would tell you which. Any of the three can be the right answer: a diagnosis the evidence supports gets *holds up*. Replies with no earlier diagnosis to test leave the line out.

## Installing

Download the latest release: [pressable-troubleshooting.zip](https://github.com/pressable/pressable-troubleshooting/releases/latest/download/pressable-troubleshooting.zip)

Both Claude and ChatGPT support skills directly. Install this the way your assistant documents:

- Claude: [Use Skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
- ChatGPT: [Skills in ChatGPT](https://help.openai.com/en/articles/20001066-skills-in-chatgpt)

Other assistants handle this differently, and the specifics change often enough that instructions kept here would go stale. Check your assistant's own documentation for how it loads skills or custom instructions.

Once it is installed you do not need to invoke it by name. Describe your problem the way you normally would.

## What Is In Here

- `SKILL.md`: purpose, core rules, and the investigation workflow.
- `references/`: detailed guidance on platform architecture, diagnostics and performance, safety and change management, security and access, support scope and routing, output formats, and handling conclusions a conversation already contains.
- `VERSION` and `release.json`: version tracking, used by the skill to notice when a newer release exists.

## Updates

When it can reach the internet, the skill checks once per conversation whether a newer release is available and mentions it if so. It will not download, install, or modify anything. Updating means copying the new files over the old ones.

## Scope

This skill does not replace Pressable Support or the [Pressable Knowledge Base](https://pressable.com/knowledgebase/).

It also does not speak for Pressable. It cannot make commitments about what Pressable will or will not do, and everything it tells you is still an AI answer. Verify anything that matters before you act on it, and particularly before you change a production site.

## Feedback

Contact Pressable Support at help@pressable.com. Reports of the skill getting something wrong are the most useful kind: in a few lines, include what it told you and what turned out to be true, not the full conversation.
