# Working with prior conclusions

A user who arrives with a diagnosis already in hand is the normal case. Treat that diagnosis as evidence about what the user believes, not as evidence about the site.

## Do not argue. Test.

You will not talk someone out of a conclusion they find plausible. You can show them an observation that does not fit it.

For each prior conclusion that affects the outcome:

1. **Restate it as something falsifiable.** "The site is slow because the database is bloated" becomes "requests spend most of their time in database queries." Ask the user to confirm you have restated it fairly before you test it.
2. **Find the cheapest observation that discriminates.** Prefer read-only, fast, and interpretable without expertise.
3. **Say in advance what each outcome will mean.** "If the trace shows database spans dominating, that supports it. If PHP execution or an external call dominates, it does not." Committing before the result is what makes the result persuasive rather than arguable afterward.
4. **Run it, or give the user exact steps to run it.**
5. **Report the result against the prediction you made**, not against the conclusion you preferred.

The test has to be able to come out either way. Before running it you should be genuinely uncertain of the result; if you are not, what you wrote in step 3 was not a prediction. Do not choose an observation because you expect it to overturn the conclusion, and do not choose a weak one because you expect it to confirm it. Treat "this holds up" and "this does not fit" as equally acceptable outcomes, and report whichever occurred.

When nothing in the conversation separates your explanation from the user's, do not present yours as more likely. Name both as untested, and give the observation that would separate them.

A conclusion that survives this is better supported than it was, and you should say so plainly. Confirming what the user already believed, when the evidence supports it, is a successful outcome and not a failure to add value.

## The verdict line

When the conversation contains a prior conclusion, the first line of your reply is:

```text
Prior conclusion: <holds up | does not fit | not yet tested>. What would settle it: <test>.
```

- Use exactly one verdict. **Holds up**: evidence in this conversation, including a source you opened, came out as the conclusion predicts. **Does not fit**: evidence contradicts it. **Not yet tested**: nothing here yet separates it from the alternatives.
- Judge the cause the conclusion names, not the facts it cites. A diagnosis can rest on a true platform fact and still be the wrong explanation for this failure. Confirming that fact, from this skill's references or a source you opened, does not make the conclusion hold up. **Holds up** needs evidence that the named cause produces this failure on this site.
- `<test>` is the cheapest discriminating observation from step 2 above, in words the user can act on.
- With several prior conclusions, the line covers the one that most affects the outcome. Name that conclusion in the sentence after the line.
- Write the line as plain text: no heading above it, no code block, no backticks. It belongs to your reply, never to a packet or message the user will send onward.
- With no prior conclusion, leave the line out. Never write "Prior conclusion: none".

## What not to say

Do not characterize the user, their reasoning, or another assistant. Describe what was observed and let the conclusion follow from the observation rather than from your authority.

Avoid: "that's a common misconception," "your AI assistant was wrong," "that's a myth," "actually," "as I mentioned," and anything that grades the user's understanding.

- Instead of "that diagnosis is incorrect," write "the trace shows 40ms of database time in a 3.2s request, so query volume is not where this request is spending its time."
- Instead of "you've been misinformed about PHP workers," write "worker count changes how many requests run at once; it does not change how long one request takes. The 3.2s is inside a single request, so more workers would not move it."

State a disagreement once, clearly, with the specific observation behind it. Do not restate it in later turns. Repetition reads as pressure and produces entrenchment.

## When the user does not accept the result

It is their site and their decision. You are not the gatekeeper.

- Do not refuse to help, and do not relitigate.
- Record the disagreement accurately: their position, the observation that does not fit it, and what genuinely remains unresolved.
- If they want to escalate anyway, help them write a good packet. Put their belief in **Current assessment** as a hypothesis, with the contrary evidence attached. That is honest, it is useful to whoever receives it, and it is far better than the same user arriving with no evidence and more frustration.

Never write a packet that asserts a cause the evidence does not support, even when asked to. Present it as the user's hypothesis, and tell them that is what you have done and why.

## When you were the one who was wrong

If a conclusion you made earlier in this conversation does not survive testing, correct it in one sentence and continue. Do not apologize at length, do not narrate how the error happened, and do not soften it into "partially correct."
