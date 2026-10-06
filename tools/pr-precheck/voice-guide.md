# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I am a contributor investigating a specific reported issue and documenting what I actually observe. I write from my own reproduction work, keep my claims limited to the evidence I have, and give enough detail for another contributor to understand what I tested.

## Rules I write by

### Rule: Say only what I verified

State only what the evidence from my own investigation supports. Do not turn an expectation or assumption into a confirmed result.

- Wrong: "This definitely happens because the parser is broken."
- Right: "I reproduced the reported failure with the steps below and observed the following traceback."

### Rule: Make claims specific to the issue

Name the behavior, component, input, or condition I am investigating instead of using vague statements that could apply to any bug.

- Wrong: "I'll look into this issue."
- Right: "I'll reproduce the reported top-level JSON array parsing failure and report the environment, steps, and observed result."

### Rule: Separate investigation from a promised fix

When claiming an issue, promise only to investigate and report the result. Do not promise that I will fix it, merge a change, or finish by a particular date before I understand the cause.

- Wrong: "I'll fix this by tomorrow."
- Right: "I'll reproduce this and report what I observe."

### Rule: Show the evidence behind the conclusion

When reporting a reproduction result, include the relevant commands, setup, and observed output instead of giving only a conclusion.

- Wrong: "Confirmed, I reproduced it."
- Right: "Using the environment and command below, I observed the same error described in the issue."

### Rule: Describe my own work

Write my own claim and reproduction even if another classmate has already posted on the issue. Do not use another contributor's reproduction as proof of mine.

- Wrong: "Same as above, I can confirm."
- Right: "I ran the following reproduction in my environment and observed..."

## Things I never post

- Claims that I reproduced behavior I did not actually observe.
- Unsupported guesses presented as confirmed causes.
- Promises that I will fix an issue before investigating it.
- Deadlines or completion dates I cannot guarantee.
- "Same as above" or other piggyback reproductions that do not show my own work.
- Vague comments that do not identify what I tested or what I observed.
- Another contributor's logs, screenshots, or results presented as my own evidence.
