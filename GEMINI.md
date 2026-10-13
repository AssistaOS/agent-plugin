# Assista

Assista writes posts in the user's own voice and keeps every post behind their
approval. This extension connects you to their Assista account.

## Tools

- `get_voice_brief`: their voice brief, banned words, lines to avoid and recent
  posts. Read it before writing anything.
- `get_interview`: this week's three questions and their answers. Draft from
  the answers.
- `get_feedback`: what they approved, edited or rejected. Do not repeat what
  they edited out.
- `submit_drafts`: sends up to three drafts to their Assista approvals, each
  with its `channel`; `interview_id` is optional. It runs their checks and the
  channel's limits first and returns the rule id of anything it refused.
- `get_brand_kit`, `submit_graphics`, `submit_video`: graphics and short
  videos in their brand, sent to their approvals. See the skill's
  `references/graphics.md` and `references/video.md`.
- `answer_interview`: saves their answers to this week's questions. Assista
  drafts nothing from them on its own; you do.
- `get_submissions`: where everything sent stands: pending, approved,
  declined, published (with its link) or failed (with the reason).
- `get_usage`: their plan, this month's posts and videos, and whether their
  own tool does the drafting.

## Rules

- There is no publish or schedule tool. Drafts wait in Assista until the person
  approves them. Never say a post was published.
- One draft per angle, plain text, no links in the body, no Markdown.
- Respect the channel's limits: LinkedIn 3,000 characters, X 280, Threads 500,
  Facebook 63,206, Instagram 2,200 with an image. The `assista-drafts` skill
  has the full rules per channel in `skills/assista-drafts/references/`.
- Finish by giving the person each saved post's `url` (its page in Assista,
  where they approve it). Assista also sends them a note.

If the tools ask for sign-in, run `/mcp auth assista`. The person signs in to
Assista in the browser; no key is needed.
