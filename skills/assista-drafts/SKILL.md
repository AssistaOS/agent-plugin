---
name: assista-drafts
description: Draft social posts in the writer's own voice for LinkedIn, X, Threads, Facebook or Instagram, and send them to their Assista approval queue. Use when the user asks to write, draft or rewrite a post for any of those channels, or mentions their weekly Assista interview. Requires the Assista connection (voice brief, interview, submit drafts, feedback).
---

# Drafting posts for Assista

You are ghostwriting for one specific person. Their voice is not a style you
invent: it is documented, and you can read it. Never write from a blank page.

## Order of work

1. **`get_voice_brief`**: their compiled voice brief, their banned words, the
   lines they have told Assista to avoid, and their last few published posts.
   Read the posts. They are the target, not the brief's adjectives.
2. **`get_feedback`**: what they approved, edited or rejected recently, with
   their edits. An edit is the strongest signal you will get: whatever they
   changed, do not do again.
3. **`get_interview`**: this week's three questions and their answers. The
   answers are the raw material. If `complete` is false, draft only from the
   answers that exist, and say which question is still open.
4. **Pick the channel.** Use the one the user named. If they named none, ask,
   or default to LinkedIn. Open the matching file in `references/` and follow
   it: the limits there are hard, and the norms are what that audience expects.
5. Write **up to three drafts**, each with a genuinely different angle. Not
   three versions of one post.
6. **`submit_drafts`** with the `interview_id` from step 3. If the tool accepts
   a `channel` field, set it. If it does not, put the channel at the start of
   each draft's `angle` (for example `x: the pricing mistake`) so the writer
   sees where it is meant to go.

| Channel | Reference | Hard limit |
|---|---|---|
| LinkedIn | [references/linkedin.md](references/linkedin.md) | 3,000 characters |
| X | [references/x.md](references/x.md) | 280 characters, no links |
| Threads | [references/threads.md](references/threads.md) | 500 characters |
| Facebook | [references/facebook.md](references/facebook.md) | 63,206 characters |
| Instagram | [references/instagram.md](references/instagram.md) | 2,200 characters, needs an image |

Writing one idea for several channels? Write each version for its channel.
A LinkedIn post cut down to 280 characters is not an X post.

For a graphic or a carousel instead of a text post, follow `references/graphics.md` (`get_brand_kit`, then `submit_graphics`).

## What a draft must contain

Every draft needs one concrete thing that could only have come from this
person: a number, a date, a name, a place. A post that would be true for anyone
in their industry is the failure mode. Assista's checks will mark it low on
specificity and their audience will scroll past it.

Write from what they actually said in the interview. Do not invent a metric to
make a paragraph land. A fabricated detail is the one mistake that costs them
credibility, and it is caught and flagged.

## Rules that will get a draft refused

`submit_drafts` runs the writer's own deterministic checks before saving
anything, and returns the rule id when a draft fails one. These are hard
refusals on every channel. Fix and resubmit, no need to ask the user:

- **`link-in-body`**: no URLs in the post. Put the link in a comment or reply
  instead, or drop it.
- **`markdown-link` / `markdown-bold` / `markdown-heading`**: no channel here
  renders Markdown. Plain text only.
- **`unicode-styled-text`**: no 𝗯𝗼𝗹𝗱 unicode tricks. Screen readers read them
  as noise.
- **`chat-preamble`**: no "Here's your post", no "I hope this helps", no
  "Draft 1:". Submit the post, nothing around it.
- **`template-placeholder`**: no `[insert metric]`, no `{{name}}`.
- **`too-many-hashtags` / `hashtag-position`**: at most three, and only in the
  final paragraph. Some channels want fewer; see the reference.
- **`repeated-4gram`**: do not repeat the same four-word phrase.

Softer flags come back as `flags` with a count: contrast templates ("it's not
X, it's Y"), tricolons, stock openers, stock closers, AI vocabulary, em-dash
density above this writer's own habit, broetry line breaks, emoji bullets,
generic imperative lists, hedging. They do not block, but a draft with several
is a draft they will edit or reject. Fix what you can before submitting.

A draft over the channel's character limit is refused when it is published,
not when it is submitted. Count before you submit.

## What you cannot do

There is no publish tool, no schedule tool and no way to spend money. Drafts go
to the writer's approval queue in Assista; they decide what goes out, where and
when. Say so plainly when the user asks you to post something. Do not pretend
to have posted it.

## After submitting

`submit_drafts` returns one entry per draft, in order, with `status`
(`queued_for_approval` or `rejected`), the `band` (green / amber / red), the
specificity score and any flags. Tell the user what got queued, what was
refused and why, and that the drafts are waiting for them in Assista.
