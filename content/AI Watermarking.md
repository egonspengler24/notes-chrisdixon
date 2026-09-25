---
title: "What AI gets wrong about knowledge work"
date: 2026-09-25
tags:
  - AI
  - strategy
---
# AI Watermarking: The Invisible Ink of the LLM Era

25 Sept 2026 · @Chris

Since August 2026, under the EU's Code of Practice on Transparency of AI-Generated Content, major model providers — Anthropic included — have started weaving statistical watermarks directly into the text their models produce. No hidden characters, no metadata to strip out: the mark lives in the words themselves. Here's what that actually means, how it works, and why it's controversial.

## Writing is just a long chain of small decisions

Every time a language model produces the next word, it isn't reaching for one "correct" answer — it's choosing from a shortlist of candidates that would all read naturally. Take "The weather today was cold and…". The model won't pick something absurd, but "overcast" and "grey" are both entirely reasonable, and which one it lands on is essentially arbitrary.

A page of generated text is full of these small forks — moments where several options are equally good, and the choice comes down to a roll of the dice. Ordinarily that dice roll is just noise. Watermarking repurposes it.

## How the mark actually gets in there

The technique Anthropic uses is a version of Google DeepMind's SynthID-Text, part of a family of approaches tracing back to a 2022 proposal by Scott Aaronson. The mechanics:

- At each "any of these words would do" moment, a secret key splits the candidate words into two arbitrary buckets — call them green and red.
- The model is nudged, gently, toward green. Not forced — a red word can still win — just made a little more likely.
- The colouring isn't fixed: the same word might be green after one run of preceding words and red after another, because the key derives the split from the local context each time.

Detection doesn't involve reading for "AI-sounding" phrasing (that's what unreliable tools like GPTZero try to do). Instead, whoever holds the key re-runs that same colouring over a piece of text and counts how often it lands green. Genuine human writing should land green about half the time — a coin flip. Text that lands green noticeably more often is very likely to have passed through the watermarking model, and the longer the passage, the more confident that verdict can be.

Anthropic is at pains to stress what this isn't: nothing is inserted into the text, it carries no information about who you are or which conversation produced it, it costs no extra tokens, and it's sparse or absent wherever there's genuinely only one right answer — factual claims, code, exact quotations.

## The bit that makes people angry

Here's where it gets contentious, and [John Gruber's response on Daring Fireball](https://daringfireball.net/2026/08/anthropics_watermark_text_adulteration_in_claude_is_a_perversion_of_writing) is the sharpest version of the objection. This isn't a metadata tag bolted onto a file, like the C2PA credentials attached to AI-generated images. It's a deliberate bias applied to the actual words chosen, for a purpose that has nothing to do with communicating clearly with the reader. Anthropic's own framing — that choosing "grey" over "overcast" doesn't matter much to the reader — is, in this view, exactly the problem: word choice is the writer's whole job, and treating word-level differences as beneath notice licenses the model to sacrifice precision, however slightly, for a purpose that serves regulators rather than the reader.

There's a sharper practical worry too: the watermark attaches to anything Claude _touches_, not just anything it originates. Ask it to proofread your own writing, and depending how heavily it edits, your work could pick up enough of a statistical signature to register as "AI-involved" later — with no way for you, or anyone without Anthropic's key, to check or contest that.

Which leads to the detection problem: only the key-holder can run the test. There's no independent, public way to verify a watermark claim. Anthropic has said a detection API is coming, initially limited to regulators, researchers and similarly obligated organisations — but for now, if a Claude watermark says something about a piece of text, you have to take that on trust.

## Can it be removed?

Yes and no. Because the mark depends on the exact sequence of original words, light editing barely touches it — published research suggests even fairly heavy paraphrasing needs a few hundred words before the accumulated changes erase the statistical signal, and one-pass rewrites tend to _dilute_ the mark rather than delete it. What genuinely removes it is a full, meaning-preserving rewrite that shares no runs of the original wording — which is arguably no longer really "the same text" at all. Tools built specifically to strip watermarks by regenerating text from its meaning (rather than lightly rewording it) already exist, and will only get better.

## Why this matters for education (and beyond)

This is the part that makes the whole topic worth writing up. A statistical, provider-side watermark sounds like exactly the tool a school or university would want: a way to check, after the fact, whether submitted work passed through an AI model. But the caveats above matter enormously in that context:

- **It can't distinguish "wrote" from "edited."** A student who used Claude only to fix grammar in their own essay could register the same "AI-involved" signal as one who had it write the whole thing, depending on length and detection thresholds.
- **Only the provider can check.** A teacher can't independently verify a claim of AI involvement — they'd need Anthropic's (or Google's) detection tooling, which isn't broadly available and won't say _how much_ was AI versus human.
- **It's beatable by the people most likely to cheat.** A determined student passing work through a rewriting tool will strip the mark; an honest student who used AI lightly and transparently is the one most likely to get flagged.
- **Short work carries almost no signal.** A one-paragraph answer may simply not contain enough "forks" for the watermark to say anything useful.

In other words, watermarking looks like it solves "did AI write this?", but what it actually offers is a probabilistic hint that's strongest exactly where it's least needed (long, unedited AI output) and weakest exactly where the incentive to cheat is highest (short, heavily edited or laundered text). Institutions that lean on it as a definitive answer rather than one weak signal among several risk both false confidence and false accusations.

## Where I've landed

This isn't quite the "poison pill" some of the more heated commentary makes it out to be — the EU's underlying goal, being able to tell when content circulating publicly was AI-generated, is a reasonable one to want _some_ answer to. But the current implementation is a genuinely awkward compromise: invisible enough to avoid degrading the reading experience, but also invisible enough that nobody outside the provider can check its claims, verify its accuracy, or contest a false positive. That combination — real effect on the text, zero visibility into how or when it's applied — is going to keep generating exactly this kind of friction as more people run into it in classrooms, editorial workflows, and anywhere else "was this AI?" carries real stakes.

## Further reading

- [Anthropic — How Claude's text watermark works](https://www.anthropic.com/news/claude-text-watermark)
- [James Padolsey / declaude — How AI text watermarking works: a visual guide](https://declaude.org/watermarking/) — the best interactive explainer of the underlying mechanics
- [John Gruber — Anthropic's 'Watermark' Text Adulteration in Claude Is a Perversion of Writing](https://daringfireball.net/2026/08/anthropics_watermark_text_adulteration_in_claude_is_a_perversion_of_writing) (Daring Fireball) — the fullest version of the critical case
- [Wikipedia — Steganography](https://en.wikipedia.org/wiki/Steganography) — background on the broader technique family
- [Computerphile video on AI text watermarking](https://youtu.be/Cmi-1QSaptA)

